# Lens - utilitario para pod's Kubernets

Este é um pequeno manual para instalação do necessário para rodar o app Lens que pode ser obitido clicando [aqui](https://github.com/lensapp/lens/releases/)

Para que este app funcione corretamente antes temos que ter utilitários como `AWS Cli 2` e `Kubectl` instalados em nosso sistema. 



### AWS Cli v2

Instalação, em seu terminal cole o comando:

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    sudo ./aws/install
```

Verifique a instalação

```bash
aws --version
```

### Kubectl

instalação, em seu terminal cole o comando:

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && \
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

Verifique a instalação 

```bash
kubectl version --client
```


