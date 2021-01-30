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



## Passo-a-Passo
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
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
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

sudo yum install git -y

```
$ echo $JAVA_HOME
```

<table>
    <tbody>
        <tr>
        <th><img align="left" width="600" src="https://objectstorage.us-ashburn-1.oraclecloud.com/n/idsvh8rxij5e/b/imagens_git/o/Captura%20de%20tela%20de%202021-01-30%2015-41-52.png"/></th>
        </tr>
    </tbody>
</table>

12. Na console do Jenkins, em Plugins Disponíveis (http://IP_OFERECIDO_CONSOLE_OCI:8080/pluginManager/available, busque por GitHub e instante sem reinciar.

<table>
    <tbody>
        <tr>
        <th><img align="left" width="600" src="https://objectstorage.us-ashburn-1.oraclecloud.com/n/idsvh8rxij5e/b/imagens_git/o/Captura%20de%20tela%20de%202021-01-30%2016-32-01.png"/></th>
        </tr>
    </tbody>
</table>








  
