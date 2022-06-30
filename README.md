# Unreal VR Template

This repository contains a series of [branches](#branches) containing specifically-targeted starter templates for Unreal Engine VR projects.

## Goal

The goal of this repository is to get a VR project successfully packaged and running on your head mounted display. It should provide a known working starting point for new VR projects. It is not focused on providing game logic or starter content.

Each branch contains a specifically-targeted template with documentation for setting up your environment.

Our templates are geared with a "bring your own" mentality aimed primarily at reducing the complexity and mystery of building and packaging for devices, as well as initializing some basic features light hand-tracking. We hope it provides a unified source of knowledge for getting started in Unreal / VR development.

## Branches

### [`main`](https://github.com/sticky/unreal-vr-template/tree/main)

- This branch, it just contains information about the repository and other branches

### [`ue4-windows-hands-bp`](https://github.com/sticky/unreal-vr-template/tree/ue4-windows-hands-bp)

- Technology:
  - Unreal Engine 4.27
  - Windows 10
  - Oculus Quest 2
- Features:
  - Hand tracking (with default hand meshes)
  - Blueprints

### [`ue5-mac-controllers-bp`](https://github.com/sticky/unreal-vr-template/tree/ue5-mac-controllers-bp)

- Technology:
  - Unreal Engine 5.0.2
  - MacOS (Monterey)
  - Oculus Quest 2
- Features:
  - Controllers (with Quest 2 Controller meshes)
  - Blueprints

## Contributions

This is an open source project and document! Despite there being a vast and active Unreal Engine community publishing tutorials and answering forum questions, we found it extremely difficult to get a clear, unified answer on everything related to VR development. The topic itself is huge and involves many moving pieces.

We created this project to answer these questions for our own needs. Ideally, they will answer your questions as well. There is still a ton that we don't fully understand about the VR development and packaging process and welcome any and all expertise.

If you would like to contribute, please create an issue or search to see if one already exists in this repository. As a reminder, this project is focused on knowledge and guidance related to VR packaging. We are not interested in provided game content or logic.
