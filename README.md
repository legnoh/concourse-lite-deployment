concourse-lite-deployment
====

concourse lite VM deploy
- [Concourse構築(Lite-VM) @ GCP - Qiita](https://qiita.com/legnoh/items/75083932334b434dfa7f)

```sh
$ git clone git@github.com:legnoh/concourse-lite-deployment.git --recursive
$ cd concourse-lite-deployment
$ direnv allow
$ bosh create-env concourse-bosh-deployment/lite/concourse.yml \
       -l concourse-bosh-deployment/versions.yml \
       -l key-creds.yml \
       -o concourse-bosh-deployment/lite/infrastructures/gcp.yml \
       -o customize.yml \
       -v internal_cidr=10.0.0.0/28 \
       -v internal_gw=10.0.0.1 \
       -v internal_ip=10.0.0.3 \
       -v mbus_bootstrap_password=$MBUS_BOOTSTRAP_PASSWORD \
       -v network=concourse-network \
       -v postgres_password=$POSTGRES_PASSWORD \
       -v project_id=$PROJECT_ID \
       -v public_ip=$PUBLIC_IP \
       -v subnetwork=concourse-subnet \
       -v tags=[] \
       -v zone=$ZONE \
       --var-file gcp_credentials_json=gcp-state.json

$ fly -t m l -c http://$PUBLIC_IP \
&& fly -t m s \
&& fly -t m st -n main --non-interactive \
   --basic-auth-username=$MAIN_USER --basic-auth-password=$MAIN_PASSWORD
```
