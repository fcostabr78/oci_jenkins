# Jenkins@OCI


## Requistos
1. Criar uma instância no Oracle Cloud: https://docs.oracle.com/en-us/iaas/Content/Compute/Tasks/launchinginstance.htm <br>

1.1 A esta instância atribuir IP Publico e deixar porta 8080 exposta (TCP traffic for ports: 8080). Selecionar Oracle Linux como a imagem no provisonamento.


<table>
    <tbody>
        <tr>
        <th><img align="left" width="600" src="https://objectstorage.us-ashburn-1.oraclecloud.com/n/idsvh8rxij5e/b/imagens_git/o/Captura%20de%20tela%20de%202021-01-30%2015-51-53.png"/></th>
        </tr>
    </tbody>
</table>



## Passo-a-Passo para Instalação do Java, Git e Jenkins

2. Acessar a VM via SSH e instalar o Java

```
$ ssh -i path/id_rsa opc@IP)
$ sudo yum install java-1.8*
```

2.1 Liberar os acecsso as porta 80 e 8080

```
$ sudo firewall-cmd --zone=public --permanent --add-port=80/tcp
$ sudo firewall-cmd --zone=public --permanent --add-port=8080/tcp
$ sudo firewall-cmd --reload
```

3. Após a instalação configure o JAVA-HOME

```
$ find /usr/lib/jvm/java-1.8* | head -n 3
```

deverá haver uma saída similar a esta


/usr/lib/jvm/java-1.8.0<br>
/usr/lib/jvm/java-1.8.0-openjdk<br>
/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.262.b10-1.el7.x86_64-debug<br>

```
$ JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.262.b10-1.el7.x86_64-debug
$ export JAVA_HOME
$ PATH=$PATH:$JAVA_HOME
$ echo $PATH
/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/opc/.local/bin:/home/opc/bin:/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.262.b10-1.el7.x86_64-debug
```

4. Altere o bash profile para manter a configuração do path

```
$ vi ~/.bash_profile 
```

5. Instale o Git

```
$ sudo yum install git -y
```

6. Instale o Jenkins

```
$ sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
$ sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
$ sudo yum -y install jenkins
```

7. Inicialize o Jenkins

```
$ sudo service jenkins star
$ sudo chkconfig jenkins on
```

8. Copie a senha
```
$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

9. Abra o navegador no IP oferecido no provisionado da VM no Oracle Cloud (passo 1), porta 8080
```
http://ip_atribuido_a_vm:8080
```

<table>
    <tbody>
        <tr>
        <th><img align="left" width="600" src="https://objectstorage.us-ashburn-1.oraclecloud.com/n/idsvh8rxij5e/b/imagens_git/o/Captura%20de%20tela%20de%202021-01-30%2015-23-56.png"/></th>
        </tr>
    </tbody>
</table>


10. Pule as configurações de plugin e troque a senha no perfil do admin


11. Na console do Jenkins, em Plugins Disponíveis (http://IP_OFERECIDO_CONSOLE_OCI:8080/pluginManager/available, busque por GitHub e instante sem reinciar.

<table>
    <tbody>
        <tr>
        <th><img align="left" width="600" src="https://objectstorage.us-ashburn-1.oraclecloud.com/n/idsvh8rxij5e/b/imagens_git/o/Captura%20de%20tela%20de%202021-01-30%2016-32-01.png"/></th>
        </tr>
    </tbody>
</table>


12. Configure o path do JDK e do GIT em Global Tool Configuration - http://IP_OFERECIDO_CONSOLE_OCI:8080/configureTools/

```
$ echo $JAVA_HOME #java_home
$ echo /usr/bin/git #path ao git
```

<table>
    <tbody>
        <tr>
        <th><img align="left" width="600" src="https://objectstorage.us-ashburn-1.oraclecloud.com/n/idsvh8rxij5e/b/imagens_git/o/Captura%20de%20tela%20de%202021-01-30%2017-17-39.png"/></th>
        </tr>
    </tbody>
</table>
<br>


## Passo-a-Passo para Instalação do Maven

1. Faça o download da versão 3.6 e descompacte

```
$ mkdir /opt/maven
$ cd /opt/maven
$ sudo wget https://www-us.apache.org/dist/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
$ sudo tar -xvzf apache-maven-3.6.3-bin.tar.gz
```

2. Edite o .bash_profile 

```
$ vi ~/.bash_profile
```

2.1 e adicione o path ao maven (M3_HOME e M3). Alterar a variável PATH como abaixo

```
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

M3_HOME=/opt/maven/apache-maven-3.6.3
M3=$M3_HOME/bin

PATH=$PATH:$HOME/.local/bin:$HOME/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/opc/.local/bin:/home/opc/bin:/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.262.b10-1.el7.x86_64-debug:$M3_HOME:$M3
export PATH

```

3. Teste a instalação


<table>
    <tbody>
        <tr>
        <th><img align="left" width="600" src="https://objectstorage.us-ashburn-1.oraclecloud.com/n/idsvh8rxij5e/b/imagens_git/o/Captura%20de%20tela%20de%202021-01-30%2018-05-32.png"/></th>
        </tr>
    </tbody>
</table>
<br>
  
4. Na console do Jenkins, em Plugins Disponíveis (http://IP_OFERECIDO_CONSOLE_OCI:8080/pluginManager/available, busque por Maven Integration e Maven Invoker e instale sem reiniciar.<br>

5. Em Global Tool Configuration configure o path (/opt/maven/apache-maven-3.6.3) ao Maven
PS: Caso esse path não esteja correto, na compilação de uma task de tipo maven, o que ocorrerá é um erro informando o seguinte: Maven doesn't have a 'lib' subdirectory in Jenkins
