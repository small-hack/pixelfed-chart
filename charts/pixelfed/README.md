# pixelfed

![Version: 0.4.0](https://img.shields.io/badge/Version-0.4.0-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: v0.12.3-nginx](https://img.shields.io/badge/AppVersion-v0.12.3--nginx-informational?style=flat-square)

A Helm chart for deploying Pixelfed on Kubernetes

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| jessebot | <jessebot@linux.com> | <https://github.com/jessebot> |

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| oci://registry-1.docker.io/bitnamicharts | postgresql | 16.3.4 |
| oci://registry-1.docker.io/bitnamicharts | valkey | 2.2.1 |

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| autoscaling.enabled | bool | `false` |  |
| autoscaling.maxReplicas | int | `100` |  |
| autoscaling.minReplicas | int | `1` |  |
| autoscaling.targetCPUUtilizationPercentage | int | `80` |  |
| externalDatabase.connection | string | `"pgsql"` | options: sqlite mysql pgsql sqlsrv |
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
| externalValkey.host | string | `"valkey"` |  |
| externalValkey.password | string | `"null"` |  |
| externalValkey.port | string | `"6379"` |  |
| externalValkey.scheme | string | `"tcp"` |  |
| fullnameOverride | string | `""` |  |
| image.pullPolicy | string | `"IfNotPresent"` | This sets the pull policy for images. |
| image.registry | string | `"ghcr.io"` |  |
| image.repository | string | `"mattlqx/docker-pixelfed"` |  |
| image.tag | string | `""` | Overrides the image tag whose default is the chart appVersion. |
| imagePullSecrets | list | `[]` | This is for the secretes for pulling an image from a private repository more information can be found here: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/ |
| ingress.annotations | object | `{}` |  |
| ingress.className | string | `""` |  |
| ingress.enabled | bool | `false` |  |
| ingress.hosts[0].host | string | `"chart-example.local"` |  |
| ingress.hosts[0].paths[0].path | string | `"/"` |  |
| ingress.hosts[0].paths[0].pathType | string | `"ImplementationSpecific"` |  |
| ingress.tls | list | `[]` |  |
| livenessProbe.httpGet.path | string | `"/api/service/health-check"` |  |
| livenessProbe.httpGet.port | string | `"http"` |  |
| nameOverride | string | `""` | This is to override the chart name. |
| nodeSelector | object | `{}` |  |
| pixelfed.account_deletion | bool | `true` | Enable account deletion (may be a requirement in some jurisdictions) |
| pixelfed.activity_pub.enabled | bool | `false` | enable ActivityPub |
| pixelfed.activity_pub.inbox | bool | `false` |  |
| pixelfed.activity_pub.outbox | bool | `false` |  |
| pixelfed.activity_pub.remote_follow | bool | `false` |  |
| pixelfed.activity_pub.sharedinbox | bool | `false` |  |
| pixelfed.admin_domain | string | `""` | domain of admin interface |
| pixelfed.app.domain | string | `""` | The domain of your server, without https:// |
| pixelfed.app.env | string | `"production"` | The app environment, keep it set to "production" |
| pixelfed.app.locale | string | `"en"` | change this to the language code of your pixelfed instance |
| pixelfed.app.name | string | `"Pixelfed"` | The name of your server/instance |
| pixelfed.app.url | string | `"https://localhost"` | change this to the domain of your pixelfed instance |
| pixelfed.atom_feeds | string | `"true"` | https://docs.pixelfed.org/technical-documentation/config/#atom_feeds |
| pixelfed.db_apply_new_migrations_automatically | bool | `false` |  |
| pixelfed.enable_config_cache | bool | `true` | Enable the config cache to allow you to manage settings via the admin dashboard |
| pixelfed.enforce_email_verification | bool | `true` | Enforce email verification |
| pixelfed.exp_emc | bool | `true` | Experimental Configuration |
| pixelfed.force_https_urls | bool | `true` | Force https url generation |
| pixelfed.image_quality | int | `80` | Set the image optimization quality, between 1-100. Lower uses less space, higher more quality |
| pixelfed.instance.contact_email | string | `""` | The public contact email for your server |
| pixelfed.instance.contact_form | bool | `false` | enable the instance contact form |
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
| pixelfed.mail.driver | string | `"smtp"` | options: "smtp" (default), "sendmail", "mailgun", "mandrill", "ses" "sparkpost", "log", "array" |
| pixelfed.mail.encryption | string | `"tls"` | mail server encryption type |
| pixelfed.mail.existingSecret | string | `""` | name of an existing Kubernetes Secret for mail credentials |
| pixelfed.mail.existingSecretKeys.host | string | `""` | key in existing Kubernetes Secret for host. If set, ignores mail.host |
| pixelfed.mail.existingSecretKeys.password | string | `""` | key in existing Kubernetes Secret for password. If set, ignores mail.password |
| pixelfed.mail.existingSecretKeys.port | string | `""` | key in existing Kubernetes Secret for port. If set, ignores mail.port |
| pixelfed.mail.existingSecretKeys.username | string | `""` | key in existing Kubernetes Secret for username. If set, ignores mail.username |
| pixelfed.mail.from_address | string | `"pixelfed@example.com"` | address to use for sending emails |
| pixelfed.mail.from_name | string | `"Pixelfed"` | name to use for sending emails |
| pixelfed.mail.host | string | `"smtp.mailtrap.io"` |  |
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
| podSecurityContext | object | `{}` |  |
| postgresql.enabled | bool | `true` | enable the bundled [postgresql sub chart from Bitnami](https://github.com/bitnami/charts/blob/main/bitnami/postgresql/README.md#parameters). Must set to true if externalDatabase.enabled=false |
| postgresql.fullnameOverride | string | `"postgresql"` |  |
| postgresql.global.storageClass | string | `""` |  |
| readinessProbe.httpGet.path | string | `"/api/service/health-check"` |  |
| readinessProbe.httpGet.port | string | `"http"` |  |
| replicaCount | int | `1` | This will set the replicaset count more information can be found here: https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/ |
| resources | object | `{}` |  |
| securityContext | object | `{}` |  |
| service.port | int | `80` | This sets the ports more information can be found here: https://kubernetes.io/docs/concepts/services-networking/service/#field-spec-ports |
| service.targetPort | int | `80` | Port to attach to on the pods. Also sets what port nginx listens on inside the container. |
| service.type | string | `"ClusterIP"` | This sets the service type more information can be found here: https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types |
| serviceAccount.annotations | object | `{}` | Annotations to add to the service account |
| serviceAccount.automount | bool | `true` | Automatically mount a ServiceAccount's API credentials? |
| serviceAccount.create | bool | `true` | Specifies whether a service account should be created |
| serviceAccount.name | string | `""` | The name of the service account to use. If not set and create is true, a name is generated using the fullname template |
| tolerations | list | `[]` |  |
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
| volumeMounts | list | `[]` | Additional volumeMounts on the output Deployment definition. |
| volumes | list | `[]` | Additional volumes on the output Deployment definition. |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.14.2](https://github.com/norwoodj/helm-docs/releases/v1.14.2)
