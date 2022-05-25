# binderhub image

The image for running binderhub itself.
Built with [chartpress][].

[chartpress]: https://github.com/jupyterhub/chartpress

## Updating requirements.txt

Dependencies are specified in `requirements.in` and `requirements.txt`,
which are managed by [`pip-compile`][].

[pip-compile]: https://pip-tools.readthedocs.io

`requirements.in` contains our loosely specified direct requirements.
This is the file we edit to change what should be in the image.

`requirements.txt` is the fully pinned exact set of packages and versions.
This file is generated by `pip-compile` and should not be edited.

Because `pip-compile` resolves `requirements.txt` with the current Python for the current platform,
it must be run on the same Python version and platform as our Dockerfile.
This can be done from anywhere, by running `pip-compile` in docker from the top-level of the binderhub repo:

```shell
docker run --rm \
    --env=CUSTOM_COMPILE_COMMAND="see README.md" \
    --volume=$PWD:/io \
    --workdir=/io/helm-chart/images/binderhub \
    --user=root \
    python:3.8-slim-buster \
    sh -c 'pip install pip-tools==6.* && pip-compile --upgrade'
```

or you can just run `CUSTOM_COMPILE_COMMAND="see README.md" pip-compile` if you are already using Python 3.8 on linux.

See the [pip-compile docs][updating] for options to control the upgrading of packages.

[updating]: https://pip-tools.readthedocs.io/en/latest/#updating-requirements