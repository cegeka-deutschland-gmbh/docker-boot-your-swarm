# Docker Boot your Swarm

Der Vortrag stellt anschaulich da, wie leicht es ist eine Docker Swarm Umgebung aufzusetzen und diese mittels Jenkins zu kontinuierlich zu bestücken. Zur Veranschaulichung wird eine Demo App deployt: https://github.com/cegeka-deutschland-gmbh/docker-demo-app

## Vagrant

Um das Aufsetzen der Umgebung zu vereinfachen liegt ein Vagrantfile bei. Um die Umgebung aufzusetzen ist ein `vagrant up` in der Kommandozeile auszuführen.

Man kann sich wie folgt in den Umgebungen anmelden:

* `vagrant ssh docker#1`
* `vagrant ssh docker#2`
* `vagrant ssh docker#3`

## Präsentationen

* [08.05.2018 @ JAX](https://jax.de/cloud-container-serverless/docker-boot-your-swarm/)
