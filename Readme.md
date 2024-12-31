after vagrant is up

Run following on each node:

echo "br_netfilter" | sudo tee /etc/modules-load.d/br_netfilter.conf



then run following

cat << EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF

in the end run

sudo sysctl --system

Verify By Following:
sysctl net.bridge.bridge-nf-call-iptables
Output should be:
net.bridge.bridge-nf-call-iptables = 1

sysctl net.bridge.bridge-nf-call-ip6tables
output should be:
net.bridge.bridge-nf-call-ip6tables = 1

lsmod | grep br_netfilter
output should be:
br_netfilter should show-up. If no output then it means its not set.



