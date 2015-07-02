> **WORK IN PROGRESS**. Bear with us while we develop Connect's docs to be as complete as possible. If you need immediate support, join our Gitter channel by clicking the link below, and we'll help as soon as we can. If you want to work with us to implement or develop Connect for you professionally, contact us on Gitter.
>
>[![Gitter](https://badges.gitter.im/anvilresearch/connect.svg)](https://gitter.im/anvilresearch/connect)

## Commands that may be implemented in the CLI rewrite

login
logout

realm:select
realm:create
realm:list
realm:get
realm:update
realm:delete
realm:replicate <target>

user:register
user:list [size [page]]
user:info <uuid | email>
user:providers <uuid | email>
user:provider <uuid | email> <provider>
user:update <uuid | email> [key value]
user:remove <uuid | email>
user:assign <uuid | email> <role>
user:revoke <uuid | email> <role>
user:access_token  <uuid | email> <scope> [<scope> [...]]
user:auth <uuid | email>                                // complete id_token token auth response
user:suspend
user:reinstate

client:register
client:list [size [page]]
client:info <uuid | uri>
client:update <uuid | uri> [key value]
client:delete <uuid | uri>
client:assign <uuid | uri> <role>
client:revoke <uuid | uri> <role>
client:token  <uuid | uri> <scope> [<scope> [...]]

scope:list
scope:get <scope>
scope:create <scope> <description>
scope:update <scope> [key value]
scope:delete <scope>

role:list
role:get
role:create
role:update
role:delete
role:permit <role> <scope>
role:forbid <role> <scope>

keys:generate
keys:rotate
keys:public --copy
keys:jwks --copy

uri:authorize
uri:connect
uri:signin

init:heroku
init:modulus
init:digitalocean
init:amazon
init:joyent
init:triton
init:flynn
init:deis

push/deploy

monitor
log:search
config
status
