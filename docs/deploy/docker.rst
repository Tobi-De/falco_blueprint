Docker
======

.. warning::

    Work in progress. To receive updates follow me on `x <https://twitter.com/tobidegnon>`_ or `mastodon <https://fosstodon.org/@tobide>`_.


.. important::

    If you are using docker and the filesystem storage make sure to add a volume to the container to persist the media files and
    If you are using caprover check out this `guide <https://gist.github.com/Tobi-De/7751c394570cbf0d7beb852304394046>`_ on how
    to serve the media files with nginx


The dockerfile is located at ``deploy/Dockerfile`` and there is no ``compose.yml`` file. The setup is a bit unorthodox and use `s6-overlay <https://github.com/just-containers/s6-overlay>`_ to run everything needed for
the project in a single container.

.. admonition:: s6-overlay
    :class: note dropdown

    `s6 <https://skarnet.org/software/s6/overview.html>`_ is an `init <https://wiki.archlinux.org/title/Init>`_ system, think like `sytstemd <https://en.wikipedia.org/wiki/Systemd>`_ and
    ``s6-overlay`` is a set of tools and utilities to make it easier to run ``s6`` in a container environnment. A common linux tool peope often use in the django eocsytem that could serve as a replacement for example is `supervisord <http://supervisord.org/>`_.
    The ``deploy/etc/s6-overlay`` file contains the all the ``s6`` configuration, it will be copied in the the container in the ``/etc/s6-overlay`` directory. When the containers starts it will run a one-shot script (in the ``s6-overlay/scrips`` folder)
    that will run the setup function in the ``__main__.py`` file, then two lon process wil be run, one for the gunicorn server and one for the django-q2 worker.



There is the github action file at ``.github/workflows/cd.yml`` that will do the actual deployment. This action is run everytime a new `git tag is pushed to the repository </the_cli/start_project/packages.html#project-versioning>`_.

CapRover
********

Assuming you already have a caprover instance setup, all you have to do here is update you gihub repository with the correct credentials.
Here is an example of the content of the ``deploy-to-caprover`` job.

.. literalinclude:: ../../{{ cookiecutter.project_name }}//.github/workflows/cd.yml
    :lines: 9-21
    :language: yaml

Checkout the the `action readme <https://github.com/adamghill/build-docker-and-deploy-to-caprover>`_ for more informations, but there is essentially two things to do, add two secrets to your github repository:

``CAPROVER_SERVER_URL``: The url of your caprover server, for example ``https://caprover.example.com``
``CAPROVER_APP_TOKEN``: This can be generated on the ``deployment`` page of your caprover app, there should be an ``Enable App Token`` button.

If you are deploying from a private repository, the is also `instructions <https://github.com/adamghill/build-docker-and-deploy-to-caprover?tab=readme-ov-file#unauthorized-error-message-on-caprover>`_ to allow caprover to pull from your private repository.

And tha't basically it, if you are a caprover user, you know the rest of the drill.

Other PAAS
**********

If you using a PAAS solution that support docker, the first thing you need to do is update the ``.github/workflows/cd.yml`` action file to build your image and push it to a registry.
If you want the  `github registry <https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry>`_ your new job might look something like
this using `this action <https://github.com/docker/build-push-action>`_:

.. code-block:: yaml

  build-and-push-image:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
    steps:
      - name: Docker meta
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ghcr.io/username/projectname # CHANGE THIS
          # generate Docker tags based on the following events/attributes
          tags: |
            type=ref,event=branch
            type=ref,event=pr
            type=semver,pattern={{version}}
            type=semver,pattern={{major}}.{{minor}}
            type=semver,pattern={{major}}
            type=sha

      - name: Set up QEMU
        uses: docker/setup-qemu-action@v3

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Login to GHCR
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.repository_owner }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Checkout Code Repository
        uses: actions/checkout@v4

      - name: Build and push
        uses: docker/build-push-action@v6
        with:
          push: true
          context: .
          file: deploy/Dockerfile
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
          cache-from: type=gha
          cache-to: type=gha,mode=max




I put up the action above based on these examples, yo can look them up to adjust the action to your needs:

- https://docs.docker.com/build/ci/github-actions/push-multi-registries/
- https://docs.docker.com/build/ci/github-actions/manage-tags-labels/

.. note::

    You can also build the image locally with ``just build-docker-image`` and then push it manually on the registry of your choice.


At this point the process will be plateform dependent, but usually you should be abloe to specify the Image to pull from and that's should be it.
Eventually in the future I might add more specific guides for some of the most popular PAAS solutions.