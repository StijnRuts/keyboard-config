
```sh
podman build -t zmkbuilder -f Dockerfile ./zmk/.devcontainer

podman run -it --rm \
  --security-opt label=disable \
  --workdir /workspaces/zmk \
  -v ./zmk:/workspaces/zmk \
  -v ./config:/workspaces/zmk-config \
  -p 3000:3000 \
  zmkbuilder /bin/bash

west init -l app/
west update

cd app

west build -d build/left -b nice_nano_v2 -- -DZMK_CONFIG=/workspaces/zmk-config -DSHIELD=corne_left
west build -d build/right -b nice_nano_v2 -- -DZMK_CONFIG=/workspaces/zmk-config -DSHIELD=corne_right

exit

cp ./zmk/app/build/left/zephyr/zmk.uf2 ./left.uf2
cp ./zmk/app/build/right/zephyr/zmk.uf2 ./right.uf2
```
