# Projet6-OC
Projet Open Classroom : Déploiement AWS
*Ce script a pour fonction le déploiement d’instances EC2 sur AWS avec une copie d’instantanés.*
************************************************
## Prérequis :
### Création d’une paire de clef Amazon AWS CLI:
Pour pouvoir nous connecter aux service AWS, il est préférable d'utiliser une paire de clef AWS CLI qui permetent une connexion en "no loggin"
Commençons avec les commandes :
<pre><code>
pip3 install awscli
pip3 install --user --upgrade awscli
</code></pre>
Maintenant allons créer l’utilisateur sur AWS:
<https://console.aws.amazon.com/iam/home?#/users>
Le compte crée doit avoir des droits administrateur, soit être au minimum membre de « EC2FullAccess ». 
La création du compte faite sélectionnez l’utilisateur crée et cliquez sur download .csv (Ou, depuis le site récupérez les valeurs (« Access Key ID » & « Secret access key »)

Retournons sur le poste ansible, pour utiliser la commande 
<pre><code>
aws configure
</code></pre>
<pre><code>c
AWS Access Key ID [***************]: 
AWS Secret Access Key [************]: 
Default region name [eu-west-3]: 
Default output format [json]:
</code></pre>

Remplissez les champs, la région en pré-selection sur ce module est Paris (eu-west-3)

Si vous avez une instance déjà ouverte, vous pouvez faire un test de fonctionnement avec la commande:
<pre><code>
aws ec2 describe-instances
</code></pre>
Si la configuration est faire, je vous invite à faire un « export » des valeurs:
<pre><code>
export AWS_ACCESS_KEY_ID=*******************
export AWS_SECRET_ACCESS_KEY=****************
</code></pre>
************************************************
### Les variables du script :
Dans le fichier Projet6.yml vous pouvez changer les variables en début de fichier :
	-Instance_type : T2.micro Il s'agit de la taille du serveur loué, vous avez différents choix avec leurs performances propre).
	-Security_group : Renseigne le compte utilisé pour la connexion sur AWS, il s'agit du compte crée avec la clef AWS CLI
	-Image : Instantané de l’instance à dupliquer sur les nouvelles machines (Comme des Virtual Machine : Windows serveur 2016, Ubuntu apache …) la configuration de l’instantané est à réaliser manuellement avant l'éxécution du script.
	-Region : eu-west-3  Vous pouvez choisir dans quelle région vos instances seront hébergés par AWS, ici nos machines sont hébergés sur Paris.
	-Count : Défini le nombre d’instances à crées avec les configuration ci dessus. 

************************************************
### Modifications et création de fichiers
Quand vous vous connectez sur les services AWS il est important de déffinir la version de python utilisée par le module
Modifiez le fichier hosts pour renseigner la ligne suivante :
<pre><code>
[local]
localhost ansible_connection=local ansible_python_interpreter=/usr/bin/python3
</code></pre>
Quand vous allez lancer le script les nouvelles IP des machines crées seront enregistrés dans un fichier log. Il est important de le créer avant l'execution du script.
Vous pouvez retrouver le chemin du fichier à utiliser dans le script à ce niveau :
<pre><code>
name: Add the newly created EC2 instance(s) to the local host group
        local_action: lineinfile 
                      path=/etc/ansible/logs/****.txt
</code></pre>
Les adresses seront remontées automatiquement dans le fichier.
(il est nécéssaire de faire un « chmod » sur le fichier log pour permettre à ansible la modification de celui-ci)

### lacement du module :
<pre><code>
ansible-playbook -i /etc/ansible/hosts /etc/ansible/roles/AWS/Projet6,yml 
</code></pre>
