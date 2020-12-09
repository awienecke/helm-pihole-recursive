# helm-pihole-recursive

This helm chart I created so that I could deploy a reverse lookup after the google resolvers failed me for the last time. I also preferred to have any additional caching above and beyond what pihole does locally for enhanced speed.

This chart was designed to go into an media server or other x86 compliant device already on your network, I just didn't like how the pihole service pretty much hijacked the whole server and didn't give an elegant solution for coexisting with others.

## Usage

Usage is like any other helm chart, make sure you have the correct config in your ~/.kube/config (run `kubectl get nodes` to check.) Once you've confirmed that's good, Make modifications to the values.yaml file.
You will need to ensure that the config directory that you define exists on the server you are deploying to, or the persistence will fail.
Once that is set up, run the following command:

    helm install -f values.yaml pihole ./

## Issues

Presently, the way I'm passing the values to pihole to communicate with the unbound service, causes one of the pihole configs to show a syntax error. I've considered making changes to the pihole image, but I don't want to maintain the image. This doesn't appear to actually break anything functional.

To get this to deploy on an ubuntu server, you'll need to run the following, other distros may not have these problems:

    sudo systemctl disable systemd-resolved
    sudo systemctl stop systemd-resolved
    sudo mv /etc/resolv.conf{,.bak}
    sudo echo "nameserver 8.8.8.8" >> /etc/resolv.conf

You will also need to expand the node port range on the server to includ down to 53, as this is assuming you are using a less than blinged out kuberenetes install, most have instructions on how to do that, I'll put some here if needed.
