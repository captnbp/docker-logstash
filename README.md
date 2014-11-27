# Logstash Dockerfile

This is a highly configurable logstash (1.4.2) image.

### Linked container running Elasticsearch

If you want to link to container running Elasticsearch rather than use the embedded Elasticsearch server:

    $ docker run -d \
      -e LOGSTASH_CONFIG_URL=<your_logstash_config_url> \
      --link <your_es_container_name>:es \
      -p 9292:9292 \
      captnbp/docker-logstash

To have the linked Elasticsearch container's `bind_host` and `port` automatically detected, you will need to create an `ES_HOST` and `ES_PORT` placeholder in the `elasticsearch` definition in your logstash config file. For example:

    output {
      elasticsearch {
        bind_host => "ES_HOST"
        port => "ES_PORT"
      }
    }

I have created an example [logstash_linked.conf](https://gist.githubusercontent.com/pblittle/0b937485fa4a322ea9eb/raw/logstash_linked.conf) which includes the `ES_HOST` and `ES_PORT` placeholders to serve as an example.

### External Elasticsearch server

If you are using an external Elasticsearch server rather than the embedded server or a linked container, simply provide a configuration file with the Elasticsearch endpoints already configured:

    $ docker run -d \
      -e LOGSTASH_CONFIG_URL=<your_logstash_config_url> \
      -p 9292:9292 \
      captnbp/docker-logstash

### Finally, verify the installation

You can now verify the logstash installation by visiting the prebuilt logstash dashboard:

    http://<your_container_ip>:9292/index.html#/dashboard/file/logstash.json

## Optional, build and run the image from source

If you prefer to build from source rather than use the [pblittle/docker-logstash][1] trusted build published to the public Docker Registry, execute the following:

    $ git clone https://github.com/captnbp/docker-logstash.git
    $ cd docker-logstash

If you are using Vagrant, start and provision a virtual machine using the provided Vagrantfile:

    $ vagrant up
    $ vagrant ssh
    $ cd /vagrant

From there, build and run a container using the newly created virtual machine:

    $ make build
    $ make <options> run

You can now verify the logstash installation by visiting the [prebuilt logstash dashboard][3] running in the newly created container.

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## License

This application is distributed under the [Apache License, Version 2.0][5].

[1]: https://registry.hub.docker.com/u/pblittle/docker-logstash
[2]: https://gist.github.com/pblittle/8778567/raw/logstash.conf
[3]: http://192.168.33.10:9292/index.html#/dashboard/file/logstash.json
[4]: http://ehazlett.github.io/applications/2013/08/28/logstash-kibana/
[5]: http://www.apache.org/licenses/LICENSE-2.0
