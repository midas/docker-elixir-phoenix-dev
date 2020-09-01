# docker-elixir-phoenix-dev

Tools for building a docker base image suitable for Elixir/Phoenix dev or building releases
within Docker containers


## Usage

### Building an image in standard mode

`bin/build --elixir-version=1.8.2 --erlang-version=21.3.2 --linux-flavor=centos --linux-version=7.7.1908`

### Building an image in FIPS mode

`bin/build --elixir-version=1.8.2 --erlang-version=21.3.2 --linux-flavor=centos --linux-version=7.7.1908 --fips`
