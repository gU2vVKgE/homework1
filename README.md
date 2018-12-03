1. 所需环境
    1. 操作系统：Mac 或 Windows 系统（需有 Bash 环境）。
    2. 安装软件：Vagrant 2.2.2、VirtualBox 5.2、Ansible 2.4.2以上。
    3. 所需虚拟机镜像：从 http://cloud.centos.org/centos/6/vagrant/x86_64/images/ 下载 CentOS 6 的 vagrant 镜像 CentOS-6-x86_64-Vagrant-1809_01.VirtualBox.box 并用 `vagrant box add centos6 CentOS-6-x86_64-Vagrant-1809_01.VirtualBox.box` 命令导入 vagrant。

2. 下载本脚本
    ```
    git clone https://github.com/gU2vVKgE/homework1.git
    ```
    
3. 部署三个虚拟环境
    1. `cd homework1`
    2. `vagrant up`，
    此时会启动三台虚拟机，分为开发环境（dev）、CI/CD 环境（pipeline）、生产环境（prod），并会自动在 dev 上安装 Java 环境、Ansible、Git、Tomcat；在 pipeline 上安装 Jenkins、Git、Ansible；在 prod 上安装Ansible、Git、Tomcat。
    ```
    dev      192.168.221.2	tomcat  http://192.168.221.2:8080	ssh 192.168.221.2:22	sshkey: ./.vagrant/machines/dev/virtualbox/private_key
    pipeline 192.168.221.3	jenkins http://192.168.221.3:8088	ssh 192.168.221.3:22	sshkey: ./.vagrant/machines/pipeline/virtualbox/private_key
    prod     192.168.221.4	tomcat  http://192.168.221.4:8080	ssh 192.168.221.4:22	sshkey: ./.vagrant/machines/prod/virtualbox/private_key
    ```
    
4. 任务一：文件修改后的即时查看
    1. 假设：开发者的源文件目录与 Github 仓库 https://github.com/gU2vVKgE/sourcecode 同步（仓库路径可在 `ansible/config.cfg` 文件中修改）。
    2. 开发者在完成对源文件的修改后，使用 `git push` 更新 Github 仓库内容，然后可执行脚本 `./refresh.sh` 来更新 dev 环境的部署，然后访问 http://192.168.221.2:8080/HomeWork 验证修改后的结果。注：在执行脚本之前，需要使用 `chmod +x refresh.sh` 命令来给予脚本可执行权限。
    
5. 任务二：CI/CD 搭建
    1. pipeline 虚拟机已安装 Jenkins 环境。后续只需要进行相关配置即可进行构建、部署等工作。
    
6. 任务三：CI/CD 部署产品
    1. 对于分开部署，可考虑把静态资源部署到 AWS S3 上。访问 https://s3.us-east-2.amazonaws.com/majun2018/football.jpg 查看部署到远程AWS S3上的图片。
    2. 可使用 Jenkins 调用 ansible playbook `ansible/s3.yml`，将静态内容部署至 AWS S3 上。需提前设置 AWS 相关密钥，以及源、目的路径等参数（在文件 `ansible/config.cfg` 中设置）；同理，也可以使用类似 ansible playbook 将其他内容部署于 prod 虚拟机上。

