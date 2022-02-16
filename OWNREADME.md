# Splunkenizer environment with Cribl automation

## Table of Contents
- [Table of Contents](#table-of-contents)
- [Support](#support)
- [Setup Splunkenizer](#setup-splunkenizer)
- [Cribl automation](#cribl-automation)

## Support
Note: This framework is not officially supported by Splunk. I am developing this on best effort in my spare time.

## Setup Splunkenizer
1. Download the `git`, `vagrant` and `ansible` linux packages
```bash
sudo apt install git
sudo apt install vagrant
sudo apt install ansible
```
2. Update `vagrant` to the required and latest version
```
sudo add-apt-repository ppa:tiagohillebrandt/vagrant
sudo apt update
vagrant plugin install vagrant-vbguest
vagrant plugin install vagrant-scp
vagrant plugin update
```
3. Create `vagrant` folder for later setup
```
mkdir /opt/Vagrant
cd /opt/Vagrant
```
4. Clone the official `Splunkenizer git` repository or mine forked one
```
git clone https://github.com/splunkenizer/Splunkenizer.git
OR
git clone https://github.com/marcvonrohr/Splunkenizer.git
```
5. Continue with step 5 within `Splunkenizer README.md`

## Cribl automation
The `cribl` installation has been implemented as a new template saved under the`examples` directory. The installation is fully automated and integrated into the existing and official `Splunkenizer git` repository. The `Splunkenizer Host List` will also be populated with the server details and links of the `cribl` instance and should be used as an entry point for connecting to the `cribl` server. The additional resources which where implemented for setting up the `cribl` automation are stored in the following files:

- modified: _ansible/plugins/inventory/splunkenizer.py_
- modified: _ansible/roles/splunk_control/templates/config/index.html.j2_
- modified: _ansible/setup_other_roles.yml_
- new files: _OWNREADME.md_
- new files: _ansible/roles/cribl_environment_reconfig/_
- new files: _ansible/roles/cribl_server/_
- new files: _examples/idx_sh_uf_cribl.yml_

Use the example configuration `idx_sh_uf_cribl.yml` to setup an environment which includes an `indexer`, `search head`, `universal forwarder` and a `cribl` server within the log flow pipeline. The deployment process for this environment is the same as described in the `Splunkenizer README.md`. Use the following credentials to authenticate on the cribl server (same as for the Splunk components):
- user: **admin**
- password: **splunklab**

The cribl server doesn't store a lot of initial configurations. Basically it provides the fulfilled setup steps of a cribl deployment. Additional it provides a configured splunk `source` and `destination`, `pipeline` and a `data route`. The `universal forwarder` and the `search head` are sending there data to the `cribl` server. The `cribl` server forwards these logs to the splunk `indexer`. The only data manipulation through the pipeline which is active on `cribl` after the setup, is the data rerouting from the `_internal` index to the `main` index. Every other `cribl` test configuration has to be done by yourself. If you want to deploy your future configuration changes, the `cribl_server role` should be used. This `role` includes all configuration files under `files` and `templates`.
