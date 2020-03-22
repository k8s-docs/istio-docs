You can gain insights into what individual components are doing by
inspecting their
`logs </docs/ops/diagnostic-tools/component-logging/>`_ or peering
inside via `introspection </docs/ops/diagnostic-tools/controlz/>`_. If
that’s insufficient, the steps below explain how to get under the hood.

The `istioctl </docs/reference/commands/istioctl>`_ tool is a
configuration command line utility that allows service operators to
debug and diagnose their Istio service mesh deployments. The Istio
project also includes two helpful scripts for ``istioctl`` that enable
auto-completion for Bash and ZSH. Both of these scripts provide support
for the currently available ``istioctl`` commands.

.. note::

   ``istioctl`` only has auto-completion enabled for
non-deprecated commands.

Before you begin
----------------

We recommend you use an ``istioctl`` version that is the same version as
your Istio control plane. Using matching versions helps avoid unforeseen
issues.

.. note::

   If you have already `downloaded the Istio
release </docs/setup/getting-started/#download>`_, you should already
have ``istioctl`` and do not need to install it again.

Install {{< istioctl >}}
------------------------

Install the ``istioctl`` binary with ``curl``:

1. Download the latest release with the command:

   .. code:: sh

      $ curl -sL https://istio.io/downloadIstioctl \| sh
   -

2. Add the ``istioctl`` client to your path, on a macOS or Linux system:

   .. code:: sh

      $ export PATH=\ :math:`PATH:`\ HOME/.istioctl/bin


3. You can optionally enable the `auto-completion
   option <#enabling-auto-completion>`_ when working with a bash or ZSH
   console.

Get an overview of your mesh
----------------------------

You can get an overview of your mesh using the ``proxy-status`` or
``ps`` command:

.. code:: sh

      $ istioctl proxy-status

If a proxy is missing from the output list it means that it is not
currently connected to a Pilot instance and so it will not receive any
configuration. Additionally, if it is marked stale, it likely means
there are networking issues or Pilot needs to be scaled.

Get proxy configuration
-----------------------

`istioctl`` </docs/reference/commands/istioctl>`_ allows you to
retrieve information about proxy configuration using the
``proxy-config`` or ``pc`` command.

For example, to retrieve information about cluster configuration for the
Envoy instance in a specific pod:

.. code:: sh

      $ istioctl proxy-config cluster [flags]

To retrieve information about bootstrap configuration for the Envoy
instance in a specific pod:

.. code:: sh

      $ istioctl proxy-config bootstrap [flags] {{< /text
>}}

To retrieve information about listener configuration for the Envoy
instance in a specific pod:

.. code:: sh

      $ istioctl proxy-config listener [flags]

To retrieve information about route configuration for the Envoy instance
in a specific pod:

.. code:: sh

      $ istioctl proxy-config route [flags]

To retrieve information about endpoint configuration for the Envoy
instance in a specific pod:

.. code:: sh

      $ istioctl proxy-config endpoints [flags] {{< /text
>}}

See `Debugging Envoy and
Pilot </docs/ops/diagnostic-tools/proxy-cmd/>`_ for more advice on
interpreting this information.

``istioctl`` auto-completion
----------------------------

{{< tabset category-name=“prereqs” >}}

{{< tab name=“macOS” category-value=“macos” >}}

If you are using the macOS operating system with the Bash terminal
shell, make sure that the ``bash-completion`` package is installed. With
the `brew <https://brew.sh>`_ package manager for macOS, you can check
to see if the ``bash-completion`` package is installed with the
following command:

.. code:: sh

      $ brew info bash-completion bash-completion: stable
1.3 (bottled)

If you find that the ``bash-completion`` package is *not* installed,
proceed with installing the ``bash-completion`` package with the
following command:

.. code:: sh

      $ brew install bash-completion

Once the ``bash-completion package`` has been installed on your macOS
system, add the following line to your ``~/.bash_profile`` file:

{{< text plain >}} [[ -r “/usr/local/etc/profile.d/bash_completion.sh”
]] && . “/usr/local/etc/profile.d/bash_completion.sh”

{{< /tab >}}

{{< tab name=“Linux” category-value=“linux” >}}

If you are using a Linux-based operating system, you can install the
Bash completion package with the ``apt-get install bash-completion``
command for Debian-based Linux distributions or
``yum install bash-completion`` for RPM-based Linux distributions, the
two most common occurrences.

Once the ``bash-completion`` package has been installed on your Linux
system, add the following line to your ``~/.bash_profile`` file:

{{< text plain >}} [[ -r “/usr/local/etc/profile.d/bash_completion.sh”
]] && . “/usr/local/etc/profile.d/bash_completion.sh”

{{< /tab >}}

{{< /tabset >}}

Enabling auto-completion
~~~~~~~~~~~~~~~~~~~~~~~~

To enable ``istioctl`` completion on your system, follow the steps for
your preferred shell:

.. warning::

   You will need to download the full Istio release
containing the auto-completion files (in the ``/tools`` directory). If
you haven’t already done so, `download the full
release </docs/setup/getting-started/#download>`_ now.

{{< tabset category-name=“profile” >}}

{{< tab name=“Bash” category-value=“bash” >}}

Installing the bash auto-completion file

If you are using bash, the ``istioctl`` auto-completion file is located
in the ``tools`` directory. To use it, copy the ``istioctl.bash`` file
to your home directory, then add the following line to source the
``istioctl`` tab completion file from your ``.bashrc`` file:

.. code:: sh

      $ source ~/istioctl.bash

{{< /tab >}}

{{< tab name=“ZSH” category-value=“zsh” >}}

Installing the ZSH auto-completion file

For ZSH users, the ``istioctl`` auto-completion file is located in the
``tools`` directory. Copy the ``_istioctl`` file to your home directory,
or any directory of your choosing (update directory in script snippet
below), and source the ``istioctl`` auto-completion file in your
``.zshrc`` file as follows:

{{< text zsh >}} source ~/_istioctl

You may also add the ``_istioctl`` file to a directory listed in the
``fpath`` variable. To achieve this, place the ``_istioctl`` file in an
existing directory in the ``fpath``, or create a new directory and add
it to the ``fpath`` variable in your ``~/.zshrc`` file.

{{< tip >}}

If you get an error like ``complete:13: command not found: compdef``,
then add the following to the beginning of your ``~/.zshrc`` file:

.. code:: sh

      $ autoload -Uz compinit $ compinit

If your auto-completion is not working, try again after restarting your
terminal. If auto-completion still does not work, try resetting the
completion cache using the above commands in your terminal.



{{< /tab >}}

{{< /tabset >}}

Using auto-completion
~~~~~~~~~~~~~~~~~~~~~

If the ``istioctl`` completion file has been installed correctly, press
the Tab key while writing an ``istioctl`` command, and it should return
a set of command suggestions for you to choose from:

.. code:: sh

      $ istioctl proxy- proxy-config proxy-status {{< /text
>}}
