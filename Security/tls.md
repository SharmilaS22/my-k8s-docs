

Symmetric - one key to encrypt and descrypt

Asymmetric
Public key
Private key

- Encrypt using private - decrypt using public (vice versa is also true)
in ssh, .pub -> public key

Same server access by different system (ssh key)
 -> by creating a public key(lock) on the server
 -> having private key to access it.

Certificates -> Validating identity -> whether they own the domain and server.
-> this prevents hackers from impersonating websites

Also certificates can be self signed or issued by a trusted Certificate Authority
CA issued certificate is secure. -> Browser has mechanism to differentiate a valid certificate from a self signed one.
-> Browser uses encryption keys to identify valid CA issued certificates

For private (internal) domains -> private offering of certificates are available within a network -> which can be installed to browsers to validate the private certificates.

The tls cert/public key of the server -> encrypted using CA's private key is sent to browser upon request.
The tls cert is validated and public key is decoded using CA's public key
Using the public key of the server, the symmetric key used for communications between the client and server is shared to the server

The client certificate is also generated for building trust and validating their identity (by the browser)

This is called PKI (Public Key Infrastructure)

Public key -> .crt or .pem
Private key -> .key or -key.pem

## In kubernetes
All components need public and private key
- Can use more than one CA

Servers (also acts as client to access other compoenents)
- Kube api server (client to kubelet and etcd)
- etcd server
- kubelet server

Client
- user(admin)
- scheduler
- kube controller manager
- Kube proxy

---
Create a Certificate for CA (Certificate Authority)

1. Generating private key
```
openssl genrsa -out ca.key 2048
```
2. Cert Signing Request using key
`KUBERNETES-CA` is the name for the cert (in logs etc)
```
openssl req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr
```
3. Get the public key/cert by **self signing**
```
openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt
```

Create a Client Certificate
1. Generate the key (same step as server)
```
openssl genrsa -out admin.key 2048
```
2. CSR,
```
openssl req -new -key admin.key -subj "/CN=kube-admin/O=system:masters" -out admin.csr
```
3. Signing (use ca.crt and ca.key for signing)
```
openssl x509 -req -in admin.csr -CA ca.crt -CAkey ca.key -out admin.crt
```
---
curl https://kube-apiserver:6443/api/v1/pods --key admin.key --cert admin.crt --cacert ca.crt

kube-config.yaml
```
apiVersion: v1
kind: Config
clusters:
- cluster:
    certificate-authority: ca.crt
    server:https://kube-apiserver:6443
  name: kubernetes
users:
- name:kubernetes-admin
  user:
    client-certificate: admin.crt
    client-key: admin.key
```

Similarly, create public and private key for scheduler, controller manager, kube proxy.

---

All the clients/servers need public CA cert (for validation)
So Totally, the public cert, private key and CA Cert is required for each service.

---

The ETCD Server (on a HA environment) has to be deployed many instances of it.
All the etcd instances need server cert/key.

These peer key pair and CA Certificate needs to provided while starting the etcd server.

---
Other names for Kube API Server(used by any client):
- kubernetes
- kubernetes.default
- kubernetes.default.svc
- kubernetes.default.svc.cluster.local
- IP addresses of the server

These names need to be mentioned in the server certificate. While creating the Cert Signing Request: use config file
```
openssl req -new -key apiserver.key -subj "/CN=kube-apiserver" -out apiserver.csr -config openssl.cnf
```
openssl.cnf
```
[alt_names]
DNS.1 = kubernetes
DNS.2 = kubernetes.default
...
IP.1 = 12.24.54.54
IP.2 = 23.45.13.45
```

When starting the kube api server, which will also act as a client to other servers like etcd, kubelet etc -> pass the client key pair generated for api server, like
- client cert/key for apiserver to access etcd server (--etcd-cafile, --etcd-certfile, --etcd-keyfile)
- client cert/key for apiserver to access kubelet server (--kubelet-certificate-authority, --kubelet-client-certificate, --kubelet-client-key)

---
Kubelet on nodes
kubelet server on each node -> needs server cert/key with the name as node_name

[kubelet-config.yaml](https://kubernetes.io/docs/reference/config-api/kubelet-config.v1beta1/)
```
kind: KubeletConfiguration
apiVersion: kubelet.config.k8s.io/v1beta1
...
```
In this above config file, the server cert/key along with CA Cert will be provided.
- The name will be system:node:node01, in the command for generating CSR, `O=system:nodes` can be assigned to give necessary permissions.

---

kubeadm:
/etc/kubernetes/manifests

---
Decode the crt and get plain text info
```
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
```
Subject: (can get name)
Subject Alternative name: (all names for the server)
Issuer, Validity

---

### Process:  for generating new admin user (user key pair)
1. User create a private key
2. Admin logs into the CA Server(where the CA cert and key are present - could be master node itself)
3. Admin uses the user's private key and generates CSR, then sign it with CA Cert and key to generate the Cert for User.
4. The cert is shared to the user which has expiration.

Instead of logging into the server to access the CA cert/key, we can use **certificate api**

User creates the CSR with their private key and creates a CertificateSigningRequest K8s Object, which can be viewed by admin and approve it using kubectl api. The object has base64 encoded key, after approving, the object has generated crt in base64 which can be decoded.

This certificate api is executed by controller manager which needs the CA siging cert and CA signing key.

---
Base64 encoding without line breaks 
```
base64 -w 0
```

---

```
k get csr

k certificate approve <user-name>

k get csr/<user-name> -o yaml

k get csr/<user-name> -o jsonpath='{.status.certificate}' | base64 -d > user.crt
```
 
kube config: (default path: .kube/config)

clusters:
- cluster info contains:
  - server name
  - client authority (file path) or client authorirty data (base64 encoded cert value)

users:
- user info contains
  - client-certificate-data or client-certificate (file path)
  - client-key-data or client-key (file path)

contexts:
- context info contains
  - user name
  - cluster name
  - namespace (optional)

current-context: context_name

```
kubectl config use-context <context-name>

# with custom file
kubectl config --kubeconfig=my-kube-config use-context <context-name>

```