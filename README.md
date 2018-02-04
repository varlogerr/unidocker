# unidocker
```
project_dir=~/Projects/my-shiny-project \
&& mkdir -p ${project_dir} \
&& cd $_ \
&& git clone https://github.com/varlogerr/unidocker \
&& cp -R docker/_manifests/. ./ \
&& mv .env_ .env
```
Edit `.env` and `docker-compose.yml` files and run `docker-compose up -d`
