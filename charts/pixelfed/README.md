# Pixelfed Helm Chart

![Version: 0.19.2](https://img.shields.io/badge/Version-0.19.2-informational?style=flat-square)  ![AppVersion: v0.12.4-nginx](https://img.shields.io/badge/AppVersion-v0.12.4--nginx-informational?style=flat-square)

A Helm chart for deploying Pixelfed on Kubernetes

* [TLDR](#tldr)
  * [Preparation for installation](#preparation-for-installation)
  * [Actually installing the chart](#actually-installing-the-chart)
* [Maintainers](#maintainers)
* [Requirements](#requirements)
  * [Sources for included subcharts](#sources-for-included-subcharts)
* [How to use persistence](#how-to-use-persistence)
* [Values (helm parameters from values.yaml)](#values-helm-parameters-from-valuesyaml)

## TLDR

### Preparation for installation

```bash
# add the chart repo to your helm repos
helm repo add pixelfed https://small-hack.github.io/pixelfed-chart

# download the values.yaml and edit it with your own values such as YOUR hostname
# especially important are pixelfed.app.domain and pixelfed.mail settings
helm show values pixelfed/pixelfed > values.yaml
```

Your `values.yaml` will need at least the following to start:

```yaml
pixelfed:
  # app specific settings
  app:
    # -- This key is used by the Illuminate encrypter service and should
    # be set to a random, 32 character string, otherwise these encrypted strings
    # will not be safe. If you don't generate one, we'll generate one for you
    # however it will change everytime you upgrade the helm chart, so it should
    # only be used for testing. In production, please set this, or pixelfed.app.existingSecret
    key: ""
    # -- use an existing Kuberentes Secret to store the app key
    # If set, ignores pixelfed.app.key
    existingSecret: ""
    # -- key in pixelfed.app.existingSecret to use for the app key
    existingSecretKey: ""
    # -- The name of your server/instance
    name: "Pixelfed"
    # -- change this to the domain of your pixelfed instance
    url: "https://localhost"
    # -- The domain of your server, without https://
    domain: ""

  # Mail Configuration (Post-Installer)
  mail:
    # -- mail server hostname
    host: smtp.mailtrap.io
    # -- mail server username
    username: ""
    # -- mail server password
    password: ""
    # -- address to use for sending emails
    from_address: "pixelfed@example.com"
    # -- name to use for sending emails
    from_name: "Pixelfed"
    # -- name of an existing Kubernetes Secret for mail credentials
    existingSecret: ""
    # keys in existing secret
    existingSecretKeys:
      # --  key in existing Kubernetes Secret for host. If set, ignores mail.host
      host: ""
      # --  key in existing Kubernetes Secret for port. If set, ignores mail.port
      port: ""
      # --  key in existing Kubernetes Secret for username. If set, ignores mail.username
      username: ""
      # --  key in existing Kubernetes Secret for password. If set, ignores mail.password
      password: ""
```

### Actually installing the chart

```bash
# install the chart
helm install --namespace pixelfed --create-namespace pixelfed/pixelfed --values values.yaml
```

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| jessebot | <jessebot@linux.com> | <https://github.com/jessebot> |

## Requirements

These are all subcharts that you can choose to install, but you can also bring your own:

| Repository | Name | Version |
|------------|------|---------|
| oci://registry-1.docker.io/bitnamicharts | mariadb | 20.2.2 |
| oci://registry-1.docker.io/bitnamicharts | postgresql | 16.4.5 |
| oci://registry-1.docker.io/bitnamicharts | valkey | 2.2.4 |

### Sources for included subcharts

- https://github.com/bitnami/charts/tree/main/bitnami/valkey
- https://github.com/bitnami/charts/tree/main/bitnami/mariadb
- https://github.com/bitnami/charts/tree/main/bitnami/postgresql

## How to use persistence

This chart has the option to use PVCs (Persistent Volume Claims) for storing data on pod restarts.

```yaml
persistence:
  # -- enable persistence for the pixelfed pod
  enabled: false
  # -- storage class name
  storageClassName: ""
  # -- size of the persistent volume claim to create. Tgnored if persistence.existingClaim is set
  storage: 2Gi
  # -- accessMode
  accessModes:
    - ReadWriteOnce
  # -- using an existing PVC instead of creating one with this chart
  existingClaim: ""
```

## Values (helm parameters from values.yaml)

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` | set affinity to specific nodes or nodegroups |
| autoscaling.enabled | bool | `false` | enable autoscaling. more information can be found [here](https://kubernetes.io/docs/concepts/workloads/autoscaling/) |
| autoscaling.maxReplicas | int | `100` | max replicas to scale up to |
| autoscaling.minReplicas | int | `1` | minimum replicas to always keep up |
| autoscaling.targetCPUUtilizationPercentage | int | `80` | CPU limit a pod needs to hit to start autoscaling new pods |
| externalDatabase.database | string | `"pixelfed"` |  |
| externalDatabase.enabled | bool | `false` | enable using an external mysql or postgresql cluster |
| externalDatabase.existingSecret | string | `""` | get database credentials from an existing Kubernetes Secret |
| externalDatabase.existingSecretKeys.database | string | `"pixelfed"` | key in existing Kubernetes Secret for database. If set, ignores externalDatabase.database |
| externalDatabase.existingSecretKeys.host | string | `""` | key in existing Kubernetes Secret for host. If set, ignores externalDatabase.host |
| externalDatabase.existingSecretKeys.password | string | `""` | key in existing Kubernetes Secret for password. If set, ignores externalDatabase.password |
| externalDatabase.existingSecretKeys.port | string | `""` | key in existing Kubernetes Secret for port. If set, ignores externalDatabase.port |
| externalDatabase.existingSecretKeys.username | string | `""` | key in existing Kubernetes Secret for username. If set, ignores externalDatabase.username |
| externalDatabase.host | string | `""` |  |
| externalDatabase.password | string | `""` |  |
| externalDatabase.port | int | `3306` |  |
| externalDatabase.username | string | `""` |  |
| externalValkey.client | string | `"phpredis"` |  |
| externalValkey.enabled | bool | `false` | enable using an external valkey or redis cluster |
| externalValkey.existingSecret | string | `""` | get valkey credentials from an existing Kubernetes Secret |
| externalValkey.existingSecretKeys.host | string | `""` | key in existing Kubernetes Secret for host. If set, ignores externalValkey.host |
| externalValkey.existingSecretKeys.password | string | `""` | key in existing Kubernetes Secret for password. If set, ignores externalValkey.password |
| externalValkey.existingSecretKeys.port | string | `""` | key in existing Kubernetes Secret for port. If set, ignores externalValkey.port |
| externalValkey.host | string | `"valkey"` |  |
| externalValkey.password | string | `"null"` |  |
| externalValkey.port | string | `"6379"` |  |
| externalValkey.scheme | string | `"tcp"` |  |
| extraContainers | list | `[]` | set sidecar containers to run along side the pixelfed container |
| extraEnv | list | `[]` | template out extra environment variables |
| extraEnvFrom | list | `[]` | template out extra environment variables e.g. from ConfigMaps or Secrets |
| extraInitContainers | list | `[]` | set extra init containers |
| extraVolumeMounts | list | `[]` | Additional volumeMounts on the output Deployment definition |
| extraVolumes | list | `[]` | Additional volumes on the output Deployment definition |
| fullnameOverride | string | `""` | This is to override the chart name, but used in more places |
| image.pullPolicy | string | `"IfNotPresent"` | This sets the pull policy for images. |
| image.registry | string | `"ghcr.io"` |  |
| image.repository | string | `"mattlqx/docker-pixelfed@sha256"` | you can see the source [ghcr.io/mattlqx/docker-pixelfed](https://ghcr.io/mattlqx/docker-pixelfed) |
| image.tag | string | `"7d1d62c8552683225456c2a552ba8ca36afb24b32f706e425310de5bf84aeab1"` | Overrides the image tag whose default is the chart appVersion (v0.12.4-nginx is currently broken due to migration errors with postgresl, so please either pin a sha tag or use dev-nging as the tag) |
| imagePullSecrets | list | `[]` | This is for the secretes for pulling an image from a private repository more information can be found here: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/ |
| ingress.annotations | object | `{}` |  |
| ingress.className | string | `""` | ingress class name, e.g. nginx |
| ingress.enabled | bool | `false` | enable deploy an Ingress resource - network traffic from outside the cluster |
| ingress.hosts[0].host | string | `"chart-example.local"` |  |
| ingress.hosts[0].paths[0].path | string | `"/"` |  |
| ingress.hosts[0].paths[0].pathType | string | `"ImplementationSpecific"` |  |
| ingress.tls | list | `[]` |  |
| livenessProbe | object | `{}` | This is to setup the liveness probe more information can be found here: https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/ |
| mariadb.auth.database | string | `"pixelfed"` | Name for a custom database to create |
| mariadb.auth.existingSecret | string | `"new-password-secret"` | Use existing secret for password details (auth.rootPassword, auth.password, auth.replicationPassword will be ignored and picked up from this secret). The secret has to contain the keys mariadb-root-password, mariadb-replication-password and mariadb-password |
| mariadb.auth.password | string | `"newUserPassword123"` | Password for the new user. Ignored if existing secret is provided |
| mariadb.auth.replicationPassword | string | `"newReplicationPassword123"` | MariaDB replication user password. Ignored if existing secret is provided |
| mariadb.auth.rootPassword | string | `"newRootPassword123"` | Password for the root user. Ignored if existing secret is provided. |
| mariadb.auth.username | string | `"pixelfed"` | Name for a custom user to create |
| mariadb.enabled | bool | `false` | enable mariadb subchart - currently experimental for this chart read more about the values: https://github.com/bitnami/charts/tree/main/bitnami/mariadb |
| nameOverride | string | `""` | This is to override the chart name. |
| nodeSelector | object | `{}` | put the pixelfed pod on a specific node/nodegroup |
| persistence.accessModes | list | `["ReadWriteOnce"]` | accessMode |
| persistence.enabled | bool | `false` | enable persistence for the pixelfed pod |
| persistence.existingClaim | string | `""` | using an existing PVC instead of creating one with this chart |
| persistence.storage | string | `"2Gi"` | size of the persistent volume claim to create. Tgnored if persistence.existingClaim is set |
| persistence.storageClassName | string | `""` | storage class name |
| phpConfigs | object | `{}` | PHP Configuration files Will be injected in /usr/local/etc/php-fpm.d |
| pixelfed.account_deletion | bool | `true` | Enable account deletion (may be a requirement in some jurisdictions) |
| pixelfed.activity_pub.enabled | bool | `false` | enable ActivityPub |
| pixelfed.activity_pub.inbox | bool | `false` |  |
| pixelfed.activity_pub.logger_enabled | bool | `false` |  |
| pixelfed.activity_pub.outbox | bool | `false` |  |
| pixelfed.activity_pub.remote_follow | bool | `false` |  |
| pixelfed.activity_pub.sharedinbox | bool | `false` |  |
| pixelfed.admin_domain | string | `""` | domain of admin interface |
| pixelfed.app.domain | string | `""` | The domain of your server, without https:// |
| pixelfed.app.env | string | `"production"` | The app environment, keep it set to "production" |
| pixelfed.app.existingSecret | string | `""` | use an existing Kuberentes Secret to store the app key If set, ignores pixelfed.app.key |
| pixelfed.app.existingSecretKey | string | `""` | key in pixelfed.app.existingSecret to use for the app key |
| pixelfed.app.key | string | `""` | This key is used by the Illuminate encrypter service and should be set to a random, 32 character string, otherwise these encrypted strings will not be safe. If you don't generate one, we'll generate one for you however it will change everytime you upgrade the helm chart, so it should only be used for testing. In production, please set this, or pixelfed.app.existingSecret |
| pixelfed.app.locale | string | `"en"` | change this to the language code of your pixelfed instance |
| pixelfed.app.name | string | `"Pixelfed"` | The name of your server/instance |
| pixelfed.app.url | string | `"https://localhost"` | change this to the domain of your pixelfed instance |
| pixelfed.atom_feeds | string | `"true"` | https://docs.pixelfed.org/technical-documentation/config/#atom_feeds |
| pixelfed.banned_usernames | string | `""` | not entirely sure if this is a list of banned usernames or text to display instead of banned usernames |
| pixelfed.covid.enable_label | bool | `true` |  |
| pixelfed.covid.label_org | string | `"visit the WHO website"` |  |
| pixelfed.covid.label_url | string | `"https://www.who.int/emergencies/diseases/novel-coronavirus-2019/advice-for-public"` |  |
| pixelfed.custom_emoji | bool | `false` | Enable custom emojis |
| pixelfed.custom_emoji_max_size | int | `2000000` | max size for custom emojis, in... bytes? |
| pixelfed.db.apply_new_migrations_automatically | bool | `false` |  |
| pixelfed.db.connection | string | `"pgsql"` | options: sqlite mysql pgsql sqlsrv |
| pixelfed.enable_config_cache | bool | `true` | Enable the config cache to allow you to manage settings via the admin dashboard |
| pixelfed.enforce_email_verification | bool | `true` | Enforce email verification |
| pixelfed.exp_emc | bool | `true` | Experimental Configuration |
| pixelfed.exp_loops | bool | `false` | exp loops (as in loops video? 🤷 |
| pixelfed.filesystem.cloud | string | `"s3"` | Many applications store files both locally and in the cloud. For this reason, you may specify a default “cloud” driver here. This driver will be bound as the Cloud disk implementation in the container. |
| pixelfed.filesystem.driver | string | `"local"` |  |
| pixelfed.force_https_urls | bool | `true` | Force https url generation |
| pixelfed.horizon.dark_mode | bool | `false` | darkmode for the web interface in the admin panel |
| pixelfed.horizon.prefix | string | `"horizon-"` | prefix will be used when storing all Horizon data in Redis |
| pixelfed.horizon.replicas | int | `1` | Number of replicas for the Horizon deployment when running separately. Ignored if autoscaling is enabled. |
| pixelfed.horizon.separate_deployment | bool | `false` | Enable running Laravel Horizon in a separate deployment. Allow to scale the backend queue workers independently. |
| pixelfed.image_driver | string | `"gd"` | library to process images. options: "gd" (default), "imagick" |
| pixelfed.image_quality | int | `80` | Set the image optimization quality, between 1-100. Lower uses less space, higher more quality |
| pixelfed.instance.contact_email | string | `""` | The public contact email for your server |
| pixelfed.instance.contact_form | bool | `false` | enable the instance contact form |
| pixelfed.instance.contact_max_per_day | int | `1` | instance contact max per day |
| pixelfed.instance.cur_reg | bool | `false` | Enable Curated Registration |
| pixelfed.instance.description | string | `"Pixelfed - Photo sharing for everyone"` | your server description |
| pixelfed.instance.discover_public | bool | `false` | Enable public access to the Discover feature |
| pixelfed.instance.landing.show_directory | bool | `true` | Enable the profile directory on the landing page |
| pixelfed.instance.landing.show_explore | bool | `true` | Enable the popular post explore on the landing page |
| pixelfed.instance.post_embeds | bool | `true` | Enable the post embed feature |
| pixelfed.instance.profile_embeds | bool | `true` | Enable the profile embed feature |
| pixelfed.instance.public_hashtags | bool | `false` | Allow anonymous access to hashtag feeds |
| pixelfed.instance.reports.email_addresses | list | `[]` | A list of email addresses to deliver admin reports to |
| pixelfed.instance.reports.email_autospam | bool | `false` | Enable autospam reports (require INSTANCE_REPORTS_EMAIL_ENABLED) |
| pixelfed.instance.reports.email_enabled | bool | `false` | Send a report email to the admin account for new autospam/reports |
| pixelfed.instance.show_peers | bool | `false` | Enable the api/v1/peers API endpoint |
| pixelfed.laravel.log_channel | string | `"stack"` | Laravel log channel. Pixelfed's default, 'stack', sends logs to the default Laravel logfile. 'stderr' allows Kubernetes to capture these logs |
| pixelfed.laravel.log_level | string | `"debug"` | logging level |
| pixelfed.mail.driver | string | `"smtp"` | options: "smtp" (default), "sendmail", "mailgun", "mandrill", "ses" "sparkpost", "log", "array" |
| pixelfed.mail.encryption | string | `"tls"` | mail server encryption type |
| pixelfed.mail.existingSecret | string | `""` | name of an existing Kubernetes Secret for mail credentials |
| pixelfed.mail.existingSecretKeys.host | string | `""` | key in existing Kubernetes Secret for host. If set, ignores mail.host |
| pixelfed.mail.existingSecretKeys.password | string | `""` | key in existing Kubernetes Secret for password. If set, ignores mail.password |
| pixelfed.mail.existingSecretKeys.port | string | `""` | key in existing Kubernetes Secret for port. If set, ignores mail.port |
| pixelfed.mail.existingSecretKeys.username | string | `""` | key in existing Kubernetes Secret for username. If set, ignores mail.username |
| pixelfed.mail.from_address | string | `"pixelfed@example.com"` | address to use for sending emails |
| pixelfed.mail.from_name | string | `"Pixelfed"` | name to use for sending emails |
| pixelfed.mail.host | string | `"smtp.mailtrap.io"` | mail server hostname |
| pixelfed.mail.password | string | `""` | mail server password |
| pixelfed.mail.port | int | `2525` | mail server port |
| pixelfed.mail.username | string | `""` | mail server username |
| pixelfed.max_account_size | int | `1000000` | The max allowed account size in KB |
| pixelfed.max_album_length | int | `6` | The max number of media per post album |
| pixelfed.max_avatar_size | int | `2000` | The max user avatar size in KB |
| pixelfed.max_bio_length | int | `256` | The max user bio length |
| pixelfed.max_caption_length | int | `1000` | The max post caption length |
| pixelfed.max_name_length | int | `32` | The max user display name length |
| pixelfed.max_photo_size | int | `15000` | The max photo/video size in KB |
| pixelfed.media_delete_local_after_cloud | bool | `true` | delete local files after saving to the cloud |
| pixelfed.media_types | string | `"image/jpeg,image/png,image/gif"` | types of media to allow |
| pixelfed.min_password_length | int | `16` | The min password length |
| pixelfed.nodeinfo | string | `"true"` | https://docs.pixelfed.org/technical-documentation/config/#nodeinfo |
| pixelfed.oauth_enabled | bool | `true` | Enable oAuth support, required for mobile/3rd party apps |
| pixelfed.open_registration | bool | `true` | Enable open registration for new accounts |
| pixelfed.pf.admin_invites_enabled | bool | `true` | Enable the Admin Invites feature |
| pixelfed.pf.enable_cloud | bool | `false` | Enable S3/Object Storage |
| pixelfed.pf.enforce_max_users | int | `2000` | in KB |
| pixelfed.pf.hide_nsfw_on_public_feeds | bool | `false` | Hide sensitive posts from public/network feeds |
| pixelfed.pf.local_avatar_to_cloud | bool | `false` | Store local avatars on S3 (Requires S3) |
| pixelfed.pf.max_collection_length | int | `100` | Max collection post limit |
| pixelfed.pf.max_domain_blocks | int | `50` | The max number of domain blocks per account |
| pixelfed.pf.max_user_blocks | int | `50` | The max number of user blocks per account |
| pixelfed.pf.max_user_mutes | int | `50` | The max number of user mutes per account |
| pixelfed.pf.max_users | int | `1000` | Limit max user registrations |
| pixelfed.pf.optimize_images | bool | `true` | Enable image optimization |
| pixelfed.pf.optimize_videos | bool | `true` | Enable video optimization |
| pixelfed.s3.access_key_id | string | `""` | s3 access_key_id. ignored if s3.existingSecretKeys.access_key_id is set |
| pixelfed.s3.bucket | string | `""` | s3 bucket |
| pixelfed.s3.endpoint | string | `""` | s3 endpoint excluding protocol such as s3.domain.com |
| pixelfed.s3.existingSecret | string | `""` | name of an existing Kubernetes Secret for s3 credentials |
| pixelfed.s3.existingSecretKeys.access_key_id | string | `""` | key in existing Kubernetes Secret for access_key_id. If set, ignores s3.access_key_id |
| pixelfed.s3.existingSecretKeys.endpoint | string | `""` | key in existing Kubernetes Secret for endpoint. If set, ignores s3.endpoint |
| pixelfed.s3.existingSecretKeys.secret_access_key | string | `""` | key in existing Kubernetes Secret for secret_access_key. If set, ignores s3.secret_access_key |
| pixelfed.s3.existingSecretKeys.url | string | `""` | key in existing Kubernetes Secret for url. If set, ignores s3.url |
| pixelfed.s3.region | string | `""` | s3 region |
| pixelfed.s3.secret_access_key | string | `""` | s3 secret_access_key. ignored if s3.existingSecretKeys.secret_access_key is set |
| pixelfed.s3.url | string | `""` | s3 url including protocol such as https://s3.domain.com |
| pixelfed.s3.use_path_style_endpoint | bool | `false` | use S3 path type instead of using a DNS subdomain |
| pixelfed.session_domain | string | `""` | domain of session? |
| pixelfed.stories_enabled | bool | `false` | Enable the Stories feature |
| pixelfed.timezone | string | `"europe/amsterdam"` | timezone for docker container |
| pixelfed.trusted_proxies | string | `"*"` | trusted proxies |
| pixelfed.webfinger | string | `"true"` | https://docs.pixelfed.org/technical-documentation/config/#webfinger |
| podAnnotations | object | `{}` | This is for setting Kubernetes Annotations to a Pod. For more information checkout: https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/ |
| podLabels | object | `{}` | This is for setting Kubernetes Labels to a Pod. For more information checkout: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/ |
| podSecurityContext.fsGroup | int | `33` | group to mount the filesystem as |
| podSecurityContext.runAsGroup | int | `33` | group to run the pixelfed pod as |
| podSecurityContext.runAsUser | int | `33` | user to run the pixelfed pod as |
| postgresql.enabled | bool | `true` | enable the bundled [postgresql sub chart from Bitnami](https://github.com/bitnami/charts/blob/main/bitnami/postgresql/README.md#parameters). Must set to true if externalDatabase.enabled=false |
| postgresql.fullnameOverride | string | `"postgresql"` |  |
| postgresql.global.storageClass | string | `""` |  |
| postgresql.volumePermissions.enabled | bool | `false` | If you get "mkdir: cannot create directory ‘/bitnami/postgresql/data’: Permission denied" error, set these (This often happens on setups like minikube) |
| readinessProbe | object | `{}` | This is to setup the readiness probe more information can be found here: https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/ |
| replicaCount | int | `1` | This will set the replicaset count more information can be found here: https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/ |
| resources | object | `{}` | set resource limits and requests for cpu, memory, and ephemeral storage |
| revisionHistoryLimit | int | `10` | how many revisions of the deployment to keep for rollbacks |
| securityContext.runAsUser | int | `33` | user to run the pixelfed container as |
| service.port | int | `80` | This sets the ports more information can be found here: https://kubernetes.io/docs/concepts/services-networking/service/#field-spec-ports |
| service.targetPort | int | `8080` | Port to attach to on the pods. Also sets what port nginx listens on inside the container. |
| service.type | string | `"ClusterIP"` | This sets the service type more information can be found here: https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types |
| serviceAccount.annotations | object | `{}` | Annotations to add to the service account |
| serviceAccount.automount | bool | `true` | Automatically mount a ServiceAccount's API credentials? |
| serviceAccount.create | bool | `true` | Specifies whether a service account should be created |
| serviceAccount.name | string | `""` | The name of the service account to use. If not set and create is true, a name is generated using the fullname template |
| tolerations | list | `[]` | set tolerations of node taints |
| valkey.auth.enabled | bool | `true` |  |
| valkey.auth.existingSecret | string | `""` |  |
| valkey.auth.existingSecretPasswordKey | string | `"password"` |  |
| valkey.enabled | bool | `true` | enable the bundled [valkey sub chart from Bitnami](https://github.com/bitnami/charts/blob/main/bitnami/valkey/README.md#parameters). Must set to true if externalValkey.enabled=false |
| valkey.fullnameOverride | string | `"valkey"` |  |
| valkey.global.storageClass | string | `""` |  |
| valkey.metrics.enabled | bool | `false` | we use a grafana exporter that logs into valkey directly, but you can enable this if you don't use that |
| valkey.persistentVolumeClaimRetentionPolicy.enabled | bool | `true` |  |
| valkey.persistentVolumeClaimRetentionPolicy.whenDeleted | string | `"Retain"` |  |
| valkey.persistentVolumeClaimRetentionPolicy.whenScaled | string | `"Retain"` |  |
| valkey.primary.disableCommands | list | `["FLUSHALL"]` | Laravel requires the ability to call FLUSHDB, which is disabled by default |
| valkey.primary.persistence.enabled | bool | `false` | enable to persistent primary data accross restarts |
| valkey.primary.persistence.existingClaim | string | `""` |  |
| valkey.replica.persistence.enabled | bool | `false` | enable to persistent replica data accross restarts |
| valkey.replica.persistence.existingClaim | string | `""` |  |
| valkey.resourcesPreset | string | `"small"` | definitions: https://github.com/bitnami/charts/blob/main/bitnami/common/templates/_resources.tpl#L15 Options: nano, micro, small, medium, large, xlarge, 2xlarge default: nano |
| valkey.tls.authClients | bool | `true` |  |
| valkey.tls.autoGenerated | bool | `false` |  |
| valkey.tls.enabled | bool | `false` |  |
