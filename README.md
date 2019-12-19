# Renovate Poetry `ensurepip is not available` Showcase

Showcase for `ensurepip is not available` error when Poetry creates a virtual environment to update `poetry.lock` file.

If Poetry is configured to create a virtual environment inside the project and Renovate calls Poetry to update a dependency, Poetry fails to create the virtual environment.

Example log output:

```json
{
  "err": {
    "killed": false,
    "code": 1,
    "signal": null,
    "cmd": "docker run --rm -v \"/mnt/renovate/gh/PicturePipe/hatch\":\"/mnt/renovate/gh/PicturePipe/hatch\" -w \"/mnt/renovate/gh/PicturePipe/hatch\" renovate/poetry poetry update --lock --no-interaction Django",
    "stdout": "Creating virtualenv hatch in /mnt/renovate/gh/PicturePipe/hatch/.venv\nThe virtual environment was not created successfully because ensurepip is not\navailable.  On Debian/Ubuntu systems, you need to install the python3-venv\npackage using the following command.\n\n    apt-get install python3-venv\n\nYou may need to use sudo with that command.  After installing the python3-venv\npackage, recreate your virtual environment.\n\nFailing command: ['/mnt/renovate/gh/PicturePipe/hatch/.venv/bin/python', '-Im', 'ensurepip', '--upgrade', '--default-pip']\n\n",
    "stderr": "",
    "message": "Command failed: docker run --rm -v \"/mnt/renovate/gh/PicturePipe/hatch\":\"/mnt/renovate/gh/PicturePipe/hatch\" -w \"/mnt/renovate/gh/PicturePipe/hatch\" renovate/poetry poetry update --lock --no-interaction Django\n",
    "stack": "Error: Command failed: docker run --rm -v \"/mnt/renovate/gh/PicturePipe/hatch\":\"/mnt/renovate/gh/PicturePipe/hatch\" -w \"/mnt/renovate/gh/PicturePipe/hatch\" renovate/poetry poetry update --lock --no-interaction Django\n\n    at ChildProcess.exithandler (child_process.js:294:12)\n    at ChildProcess.emit (events.js:198:13)\n    at ChildProcess.EventEmitter.emit (domain.js:448:20)\n    at maybeClose (internal/child_process.js:982:16)\n    at Process.ChildProcess._handle.onexit (internal/child_process.js:259:5)"
  }
}
```

The important part in `stdout` seems to be:

```console
The virtual environment was not created successfully because ensurepip is not
available.  On Debian/Ubuntu systems, you need to install the python3-venv
package using the following command.

    apt-get install python3-venv
```

The `python3-venv` package is required because Poetry [creates the virtual environment](https://github.com/python-poetry/poetry/blob/1.0.0/poetry/utils/env.py#L104) using the `venv` module.
