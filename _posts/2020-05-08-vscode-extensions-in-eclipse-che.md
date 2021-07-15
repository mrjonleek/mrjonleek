---
title: "Visual Studio Extensions in Eclipse Che"
date: 2020-05-08 00:00:00
description: Cloud-native applications are the new hot-topic. But, what about cloud-native development? One of my clients moving towards "developing in the cloud" using CodeReady Workspaces, Red Hat's productized version of Eclipse Che, and they needed a Visual Studio Code extension. Here's how we got it working for them.
featured_image: '/images/blog/eclipse-che-workspace.jpg'
---

## Introduction

We're all aware of the buzz about cloud-native applications. But a new trend is emerging: **cloud-native development**. One of my customers is moving towards "developing in the cloud" but they rely on a specific Visual Studio Code extension that scans code for security vulnerabilities. Fortunately, Visual Studio Extensions are compatible with Eclipse Che version 7.9.

In this short article, we'll describe how to port the [Vim Visual Studio Code extension][1] to an Eclipse Che plug-in and add it to a Workspace.

Note: My customer is using [CodeReady Workspaces][2], an enterprise ready Red Hat supported version of [Eclipse Che][3] for OpenShift.

Let's get started!

## Creating the Plug-in Wrapper

The Eclipse Che API is compatible with Visual Studio Code. Plug-ins are isolated and provide their own dependencies packaged in containers and run as sidecars to the pod.

We'll start by creating a new public repository to store our plug-in source code and then define our plug-in meta data.

1.  Let's start by visiting the template repository on GitHub: [mrjonleek/che-plugin-blueprint][4].
2.  Click the green button labeled "**Use this template**" near the **"Clone or Download"** button.
3.  Specify a **Repository name** and be sure the repository is set to **public**.
4.  Click to view the `meta.yaml` file source.
5.  Edit the `meta.yaml` file in GitHub by clicking the **pencil** button. ![git-edit](/images/blog/git-edit.png)
6.  There are two instances of each the following values that you will need to replace with your data: `publisherName`, `pluginName`, and `pluginVersion`. Make sure the `extensions` repository maps to the Git repository you're working in. Commit the chagnes to your `master` branch once complete.

```yaml
apiVersion: v2
## Name of the publisher.
## Example: mrjonleek
publisher: publisherName
## Slugified version of the plug-in name.
## Example: vscode-vim
name: pluginName
## For the sake of organization & simplicity
## match this to the version of the VS Code
## extension. Example: 1.14.1
version: pluginVersion
type: VS Code Extension
displayName: Vim
title: Visual Studio Code Vim
description: VS Code Vim extension ported for Eclipse Che!
icon: https://www.eclipse.org/che/images/logo-eclipseche.svg
repository: https://github.com/VSCodeVim/Vim.git
spec:
  containers:
    - image: "registry.redhat.io/codeready-workspaces/plugin-openshift-rhel8"
      name: crw-ocp-rel8
  extensions:
    ## Use the information for your repository.
    ## The pluginVersion will match a tagged release.
    ## Example: v1.14.1
    - https://github.com/publisherName/pluginName/raw/pluginVersion/vscode-vim.vsix
```

## The VS Code Extension

The Visual Studio Code Extension is a `.vsix` file that we will download from the Visual Studio marketplace and add to our repository.

1.  Visit the [Vim extension][5] on the Visual Studio Code marketplace.
2.  Click the **"Download Extension"** link located in the right most column, save it to your local hard drive and rename the file to `vscode-vim.vsix`.
3.  Return to the root of your Git repository and click the **"Upload files"** button.
4.  Upload the `vscode-vim.vsix` file and commit it to your `master` branch.
5.  Now we will tag a release by clicking **"0 releases"** on our repository dashboard.
6.  Specify the version number that you provided for the `extensions` repository in the `meta.yaml` file (ex. v1.14.1) and click the green **"Publish release"** button.
7.  Lastly, copy the link to your tagged release of the `meta.yaml` file by navigating to **Version tag → meta.yaml** and then clicking the **"Raw"** button. Copy the URL from the address bar.

## Installing the Plug-in

Now we'll install the ported plugin to your workspace.

1.  Select a workspace from your dashboard and navigate to the **"Devfile"** tab.
2.  Locate the `components:` and add a new component.

```yaml
components:
    ...
    - type: chePlugin
      reference: pasteYourPluginRepo # ex: https://raw.githubusercontent.com/readyhat/che-plugin-vim/v1.14.1/meta.yaml
```
3.  Click **"Save"** and then click **"Run"** before opening the workspace.
4.  If you're able to use Vim commands then your plugin was successfully ported and installed!

Note: You can now find the Vim plug-in in the plug-in panel by navigating to **View → Plugins** and filtering by "Show Installed Plugins".

[1]: https://marketplace.visualstudio.com/items?itemName=vscodevim.vim
[2]: https://www.redhat.com/en/technologies/jboss-middleware/codeready-workspaces
[3]: https://www.eclipse.org/che/
[4]: https://github.com/mrjonleek/che-plugin-blueprint
[5]: https://marketplace.visualstudio.com/items?itemName=vscodevim.vim
