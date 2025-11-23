# docker-ida

![Docker IDA](docker-ida-logo.png)
---

Prebuilt Dockerized Linux IDA 9.x!

Prebuilt image also available (see Usage)

## Features

- Tested on IDA 9.0 ~ 9.2
- `uv` manages IDAPython environment
- `idalib` installed
- Xvfb builtin
- Bypassed naggy EULA dialogs
- Prebuilt leaked IDA version available at [ghcr.io/nyamisty/docker-ida](https://github.com/NyaMisty/docker-ida/pkgs/container/docker-ida)

## Usage

- Select one version to begin with
  ```
  docker pull ghcr.io/nyamisty/docker-ida:9.0sp1
  
  docker pull ghcr.io/nyamisty/docker-ida:9.1
  
  docker pull ghcr.io/nyamisty/docker-ida:9.2
  ```

- Basic info
  - IDA always located at /opt/ida

## Credits
- Prebuilted versions are leaked IDA Pro 9.0/9.1/9.2
    - IDA 9.0sp1: https://xjepeflts2qwhqb77bw5vj6xguve56rpyeoxibztd7t7fehxxowt5fad.onion
    - IDA 9.1: https://xjepeflts2qwhqb77bw5vj6xguve56rpyeoxibztd7t7fehxxowt5fad.onion
    - IDA 9.2: https://xjepeflts2qwhqb77bw5vj6xguve56rpyeoxibztd7t7fehxxowt5fad.onion
