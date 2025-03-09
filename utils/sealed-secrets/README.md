1. Install kubeseal
2. Create a secret file
3. Run `kubeseal < secretfile.yaml > sealedsecretfile.yaml`

Important Operations for Sealed Secrets.

1. Updating Sealed Secrets
   To update a Sealed Secret, you must re-encrypt and apply the new Sealed Secret.
   There’s no direct update operation due to the one-way nature of encryption.
   So, do your needed changes in the original secret YAML file, then re-generate the sealed secret out of it again using a command similar to the following:

$ kubeseal < mysql-secret.yaml > mysql-sealedsecret.yaml

2. Getting the Public Key.
   To securely manage secrets with Sealed Secrets in Kubernetes, you need the public key from the Sealed Secrets controller.
   This key allows you to encrypt secrets into Sealed Secrets, which can then be safely committed to version control systems.
   Here’s how to retrieve the public key and use it to generate a Sealed Secret directly without direct access to the Kubernetes cluster.

The public key can be extracted from your Kubernetes cluster where the Sealed Secrets controller is installed.
Run the following command to save the public key to a file:

$ kubeseal --fetch-cert > mypublickey.pem
This command connects to the Sealed Secrets controller service and saves the public key to mypublickey.pem.

Now, you can directly use this public key to generate a sealed secret. Example of the command below:

$ kubeseal --cert mypublickey.pem < mysql-secret.yaml > mysql-sealedsecret.yaml

With the public key and your secret prepared, you can now generate a Sealed Secret. This can be done without direct access to the Kubernetes cluster by using the public key file (mypublickey.pem).

3. Backup and Restore.
   Backup the Sealed Secrets controller’s private key regularly. This key is crucial for decrypting Sealed Secrets, especially when migrating or restoring your Kubernetes cluster.

$ kubectl get secret -n kube-system -l sealedsecrets.bitnami.com/sealed-secrets-key -o yaml > sealed-secrets-key.backup.yaml
To restore, apply your backup key to the cluster:

$ kubectl apply -f sealed-secrets-key.backup.yaml 4. Viewing Encrypted Data.
Directly viewing encrypted data within a Sealed Secret is not possible due to its encrypted nature.
To view the secret’s content, you’d decrypt it by applying it to a cluster where the Sealed Secrets controller is installed or using alternative decryption methods that involve the controller’s private key.
