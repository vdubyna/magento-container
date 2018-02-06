# Magento container

Container does not includ the database container. Database should be on local(host) environmen.

### Requirements

* Docker
* Docker Compose
* Unison https://github.com/hnsl/unox

### Setup (MacOS)

1. Put `docker-compose.yml` example into magento root directory
2. Execute the following command `docker-compose up`
3. Configure local.xml/env.php file to use `docker.for.mac.localhost` host
4. Run the unison filesync

```
# Magento 1
unison . socket://localhost:5000/ -ignore "Path media/catalog/product/cache" -ignore 'Regex .*\.idea|git|sass-cache.*' -ignore "Path .DS_Store" -ignore "Path ._.DS_Store" -auto -batch -repeat watch
# Magento 2
unison . socket://localhost:5000/ -ignore "Path pub/media/catalog/product/cache" -ignore "Path .DS_Store" -ignore "Path ._.DS_Store" -ignore 'Regex .*\.idea|git|sass-cache.*' -auto -batch -repeat watch
```
