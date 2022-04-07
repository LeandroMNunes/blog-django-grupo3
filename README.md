1 - criar o projeto no openshift
gr3-project
oc project gr-project

2 - Instalar a aplicação no openshift via +Add (na console de Developer) - instalar via git e utilizar
o link do passo 1 (Build da app django de blog - python)
oc new-app python:latest~https://github.com/LeandroMNunes/blog-django-grupo3 --name blog-grupo3
oc expose svc/blog-grupo3

3 - Deploy do banco de dados
oc new-app postgresql-persistent --name blog-database --param DATABASE_SERVICE_NAME=blog-database --param POSTGRESQL_USER=sampledb --param POSTGRESQL_PASSWORD=sampledb --param POSTGRESQL_DATABASE=sampledb
oc set env deployment/blog-django-py-fase-4-git DATABASE_URL=postgresql://sampledb:sampledb@blog-database:5432/sampledb

4 - Criação do usuário para logar e criar o blog
Na linha de comando oc digitar o seguinte:
oc get pods --selector deployment=blog-grupo3 -o name
 - Va listar o pod com o parametro da aplicação django instalada 
 (ex: pod/blog-grupo3-f9466d8b4-jqbb4
      pod/blog-grupo3-f9466d8b4-p5zjx)
	  Copie o nome do pod - neste exemplo, temos dois PODs, selecionaremos 1:

oc exec -it blog-grupo3-f9466d8b4-jqbb4 -- /bin/bash
 - Este comando executa instruções linux direto no POD selecionar

Executar os comandos seguintes para executar a criação do wizard de usuário
cd
cp .s2i/action_hooks/setup .
./setup

5 - Na console de administração Workloads/Horizontal Pod AutoSacalers criar um novo HPA (Cluster automatico para a app)

6 - Customizando
No git, alterado arquivo
blog-django-grupo3/katacoda/settings.py
