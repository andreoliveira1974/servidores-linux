## Instalação OpenLDAP no servidor Ubuntu 24.04

Neste tutorial, você aprenderá como instalar o OpenLDAP Server no Ubuntu 24.04.  O OpenLDAP Software  é uma implementação de código aberto do  Lightweight  Directory Access  Protocol ( LDAP  ), que é um protocolo cliente-servidor leve para acessar serviços de diretório, especificamente serviços de diretório baseados em X. 500.
Referência e créditos do site [***Kifarunix.com***](https://kifarunix.com/install-openldap-server-on-ubuntu-24-04/#installing-open-ldap-server-on-ubuntu-24-04).

### Comandos iniciais
Os repositórios padrões do Ubuntu 24.04 fornecem a versão de lançamento mais recente do OpenLDAP. No momento em que este artigo foi escrito, o OpenLDAP 2.6.7 é a versão estável atual, conforme a [página de lançamento](https://www.openldap.org/software/release/).

| ***comando***
```bash
sudo apt-cache policy slapd
```
| ***saída do comando***
```bash
slapd:
  Installed: (none)
  Candidate: 2.6.7+dfsg-1~exp1ubuntu1
  Version table:
     2.6.7+dfsg-1~exp1ubuntu1 500
        500 http://archive.ubuntu.com/ubuntu noble/main amd64 Packages

```

| ***comando*** _Atualização dos pacotes_
```bash
sudo apt update && sudo apt upgrade
```


### 1 - Instalar e configuração OPENLDAP
- 1.1 - Instalar o OpenLDAP no Ubuntu 24.04 usando o comando abaixo:

| ***Comando***
```bash
sudo apt install slapd ldapscripts
```
_Durante a instalação, você será solicitado a definir a senha administrativa do OpenLDAP_
![Alt imagem de configuração SLAPD](https://kifarunix.com/wp-content/uploads/2024/02/openldap-admin-pass.png?v=1708798111&ezimgfmt=ng:webp/ngcb3)
[_fonte Kifarunix_](https://kifarunix.com/install-openldap-server-on-ubuntu-24-04/#installing-open-ldap-server-on-ubuntu-24-04)

- 1.2 - Exibir configurações do banco de dados LDAP
Você pode verificar as configurações padrão do banco de dados OpenLDAP usando o comando slapcat:

| ***Comando***
```bash
slapcat
```
|  ***saída do comando***
```bash
dn: dc=nodomain
objectClass: top
objectClass: dcObject
objectClass: organization
o: nodomain
dc: nodomain
structuralObjectClass: organization
entryUUID: fa7581ea-6787-103e-9c50-bf1e7fcd254f
creatorsName: cn=admin,dc=nodomain
createTimestamp: 20240224174335Z
entryCSN: 20240224174335.131130Z#000000#000#000000
modifiersName: cn=admin,dc=nodomain
modifyTimestamp: 20240224174335Z
```
#### Com base na saída de configuração do banco de dados SLAPD acima:
- O DN base está definido como `dn: dc= nodomain`
- O nome da organização está definido como  `o: nodomain`
- A entrada DN base do administrador LDAP está definida como  `cn=admin,dc=nodomain`

### 2 - Atualizar banco de dados OpenLDAP
Com base na configuração da sua organização, você precisa atualizar o banco de dados OpenLDAP.

Portanto, você precisa reconfigurar o pacote slapd conforme mostrado abaixo e seguir as instruções.

| ***Comando***
```bash
sudo dpkg-reconfigure slapd
```
Quando o comando for executado, você será perguntado se deseja omitir a configuração do servidor OpenLDAP. Selecione  **Não**  para que a configuração seja criada para você.

![Alt imagem de configuração SLAPD](https://kifarunix.com/wp-content/uploads/2024/02/create-openldap-initial-config.png?v=1708798154&ezimgfmt=ng:webp/ngcb3)
[_fonte Kifarunix_](https://kifarunix.com/install-openldap-server-on-ubuntu-24-04/#installing-open-ldap-server-on-ubuntu-24-04)

______
Atualize o nome de domínio DNS para construir o DN base do diretório LDAP.

![Alt imagem de configuração SLAPD](https://kifarunix.com/wp-content/uploads/2024/02/base-dn-domain-name.png?v=1708798412&ezimgfmt=ng:webp/ngcb3)
[_fonte Kifarunix_](https://kifarunix.com/install-openldap-server-on-ubuntu-24-04/#installing-open-ldap-server-on-ubuntu-24-04)
_____

Defina o nome da sua organização para uso no DN base.

![Alt imagem de configuração SLAPD](https://kifarunix.com/wp-content/uploads/2024/02/organization-name.png?v=1708798180&ezimgfmt=ng:webp/ngcb3)
[_fonte Kifarunix_](https://kifarunix.com/install-openldap-server-on-ubuntu-24-04/#installing-open-ldap-server-on-ubuntu-24-04)

____

Redefina a senha do administrador do OpenLDAP.

![Alt imagem de configuração SLAPD](https://kifarunix.com/wp-content/uploads/2024/02/openldap-admin-pass.png?v=1708798111&ezimgfmt=ng:webp/ngcb3)
[_fonte Kifarunix_](https://kifarunix.com/install-openldap-server-on-ubuntu-24-04/#installing-open-ldap-server-on-ubuntu-24-04)

_______

Escolha se deseja remover o banco de dados OpenLDAP sempre que você limpar o pacote OpenLDAP, slapd.

![Alt imagem de configuração SLAPD](https://kifarunix.com/wp-content/uploads/2024/02/purge-database-when-slapd-is-removed.png?v=1708798227&ezimgfmt=ng:webp/ngcb3)
[_fonte Kifarunix_](https://kifarunix.com/install-openldap-server-on-ubuntu-24-04/#installing-open-ldap-server-on-ubuntu-24-04)

______


Remova os arquivos de configuração antigos do banco de dados OpenLDAP para finalizar a reconfiguração. O banco de dados antigo é armazenado em  _/var/backups_.

![Alt imagem de configuração SLAPD](https://kifarunix.com/wp-content/uploads/2024/02/remove-old-database.png?v=1708798237&ezimgfmt=ng:webp/ngcb3)
[_fonte Kifarunix_](https://kifarunix.com/install-openldap-server-on-ubuntu-24-04/#installing-open-ldap-server-on-ubuntu-24-04)

_______

### 2.1 - Verifique o banco de dados OpenLDAP novamente após a reconfiguração.

| ***Comando***

```bash
slapcat
```

| ***saída do comando***

```bash
dn: dc=ldap,dc=kifarunix,dc=com
objectClass: top
objectClass: dcObject
objectClass: organization
o: kifarunix.com
dc: ldap
structuralObjectClass: organization
entryUUID: 35f90bca-678c-103e-86f7-25bd8e4e56dc
creatorsName: cn=admin,dc=ldap,dc=kifarunix,dc=com
createTimestamp: 20240224181352Z
entryCSN: 20240224181352.965664Z#000000#000#000000
modifiersName: cn=admin,dc=ldap,dc=kifarunix,dc=com
modifyTimestamp: 20240224181352Z
```

Para visualizar o RootDN, execute o comando abaixo

| ***Comando***

```bash
ldapsearch -H ldapi:/// -Y EXTERNAL -b "cn=config" "(olcRootDN=*)"
```
| ***saída do comando***

```bash
SASL/EXTERNAL authentication started
SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
SASL SSF: 0
# extended LDIF
#
# LDAPv3
# base  with scope subtree
# filter: (olcRootDN=*)
# requesting: ALL
#

# {0}config, config
dn: olcDatabase={0}config,cn=config
objectClass: olcDatabaseConfig
olcDatabase: {0}config
olcAccess: {0}to * by dn.exact=gidNumber=0+uidNumber=0,cn=peercred,cn=external
 ,cn=auth manage by * break
olcRootDN: cn=admin,cn=config

# {1}mdb, config
dn: olcDatabase={1}mdb,cn=config
objectClass: olcDatabaseConfig
objectClass: olcMdbConfig
olcDatabase: {1}mdb
olcDbDirectory: /var/lib/ldap
olcSuffix: dc=ldap,dc=kifarunix,dc=com
olcAccess: {0}to attrs=userPassword by self write by anonymous auth by * none
olcAccess: {1}to attrs=shadowLastChange by self write by * read
olcAccess: {2}to * by * read
olcLastMod: TRUE
olcRootDN: cn=admin,dc=ldap,dc=kifarunix,dc=com
olcRootPW: {SSHA}+fuCaVAvF5wkhXdzsQzGyj9/YWu+kVRB
olcDbCheckpoint: 512 30
olcDbIndex: objectClass eq
olcDbIndex: cn,uid eq
olcDbIndex: uidNumber,gidNumber eq
olcDbIndex: member,memberUid eq
olcDbMaxSize: 1073741824

# search result
search: 2
result: 0 Success

# numResponses: 3
# numEntries: 2
```

Para testar a conexão com o servidor LDAP, use o  comando `ldapwhoami` conforme mostrado abaixo.

| ***Comando***

```bash
ldapwhoami -H ldapi:/// -x
```

| ***saída do comando***

```bash
anonymous
```

A saída esperada é  `anonymous` se a conexão com o servidor LDAP está correta, já que o teste é executado sem efetuar login no servidor LDAP.

### 3 - Configurar OpenLDAP com SSL/TLS
Neste guia, usaremos certificados autoassinados. Você também pode usar certificados SSL/TLS comerciais da sua CA confiável, se os tiver.

[OpenSSL](https://www.openssl.org/) é uma implementação de código aberto dos protocolos SSL e TLS. A biblioteca
(escrita na linguagem C) implementa as funções básicas de criptografia e disponibiliza
várias funções utilitárias. Também estão disponíveis wrappers que permitem o uso desta
biblioteca em várias outras linguagens.

O [OpenSSL](https://www.openssl.org/) está disponível para a maioria dos sistemas do tipo Unix, incluindo Linux,
Mac OS X, as quatro versões do BSD de código aberto e também para o Microsoft Windows.
O OpenSSL é baseado no SSLeay de Eric Young e Tim Hudson. O [OpenSSL](https://www.openssl.org/) é utilizado para
gerar certificados de autenticação de serviços/protocolos em servidores (servers).

O Transport Layer Security (TLS), assim como o seu antecessor Secure Sockets Layer
(SSL), é um protocolo de segurança projetado para fornecer segurança nas comunicações
sobre uma rede de computadores. Várias versões do protocolo encontram amplo uso em
aplicativos como navegação na web, email, mensagens instantâneas e voz sobre IP (VoIP).
Os sites podem usar o TLS para proteger todas as comunicações entre seus servidores e
navegadores web.

A autoridade de certificação (CA), também conhecida como uma Autoridade de Certificação
Raiz, é uma empresa ou organização que atua para validar as identidades (como sites,
endereços de email, empresas ou pessoas físicas) e vinculá-las a chaves criptográficas
através da emissão de documentos eletrônicos conhecidos como certificados digitais.

Para configurar o servidor OpeLDAP com certificado SSL/TLS, você precisa de uma `CA certificate`, de um servidor `certificate` e um arquivo `certificate key`.

### 3.1 - Gerar arquivos de certificado SSL/TLS

A estrutura de diretórios da CA (Certificate Authority) e dos Certificados no Ubuntu Server

#### Apresentando a estrutura de diretórios da CA e dos Certificados Assinados

|Diretório |  Especificação padrão das configurações do OpenSSL |
| ----|-----|
|/etc/ssl/ |  Diretório padrão das configurações do OpenSSL |
|/etc/ssl/certs/| Diretório das CAs Confiáveis do Ubuntu Server
|/etc/ssl/crl/| Diretório de Requisição de Revogação de Certificados
|/etc/ssl/conf/| Diretório dos Arquivos de Configuração da CA e dos Certificados
|/etc/ssl/newcerts/| Diretório de Criação dos Novos Certificados Assinados
|/etc/ssl/private/| Diretório das Chaves Públicas e Privadas dos Certificados Assinados
|/etc/ssl/request/|Diretório das Requisições de Certificados Assinados




### 3.2 - Crie um diretório para armazenar os certificados do OpenLDAP


| ***Comando***

```bash
sudo mkdir -p /etc/ssl/openldap/{private,certs,conf,newcerts}
```

Por padrão o diretório para armazenar certificados é definido no arquivo de configuração `/usr/lib/ssl/openssl.cnf`, depois de criarmos os diretórios acima, vamos personalizar um arquivo de configuração a ser usado para o openldap especifico para nosso DOMÍNIO LDAP.

Faça uma cópia backup do arquivo de configuração padrão `/etc/lib/ssl/openssl.cnf` e defina o diretório para armazenar certificados e chaves SSL/TLS na seção  `[ CA_default ]`

| ***Comando***

```bash
sudo cp /etc/lib/ssl/openssl.cnf /etc/lib/ssl/openssl.cnf.bkp
```

#### 3.3 - Editar arquivo `openssl.cnf` definir armazenamento dos certificados

| ***Comando***

```bash
sudo vim /etc/lib/ssl/openssl.cnf
```

| ***Incluir dir = /etc/ssl/openldap*** 

```bash
...
####################################################################
[ CA_default ]

#dir            = ./demoCA              # Where everything is kept
dir             = /etc/ssl/openldap
certs           = $dir/certs            # Where the issued certs are kept
crl_dir         = $dir/crl              # Where the issued crl are kept
database        = $dir/index.txt        # database index file.
#unique_subject = no                    # Set to 'no' to allow creation of
                                        # several certs with same subject.
new_certs_dir   = $dir/newcerts         # default place for new certs.
...
```

| ***Editar `default_days	= 3650`*** 

```bash
# Extensions to add to a CRL. Note: Netscape communicator chokes on V2 CRLs
# so this is commented out by default to leave a V1 CRL.
# crlnumber must also be commented out to leave a V1 CRL.
# crl_extensions	= crl_ext

default_days	= 365			# how long to certify for
default_crl_days= 30			# how long before next CRL
default_md	= default		# use public key default MD
preserve	= no			# keep passed DN ordering

```

| ***Editar os seguintes campos com os dados de seu SERVIDOR, conforme exemplo:***

>countryName_default=BR

>stateOrProvinceName_default=Brasil

>localityName_default=SJDR

>organizationName_default=UFSJ

>organizationalUnitName_default=DCOMP

 Incluir`commonName_default` abaixo do campo `commonName= Common Name (e.g. server FQDN or YOUR name)`
>commonName_default=ldap.dcomp.local

>emailAddress_default=seu email

```bash
[ req_distinguished_name ]
countryName			= Country Name (2 letter code)
countryName_default		= AU
countryName_min			= 2
countryName_max			= 2

stateOrProvinceName		= State or Province Name (full name)
stateOrProvinceName_default	= Some-State

localityName			= Locality Name (eg, city)

0.organizationName		= Organization Name (eg, company)
0.organizationName_default	= Internet Widgits Pty Ltd

# we can do this but it is not needed normally :-)
#1.organizationName		= Second Organization Name (eg, company)
#1.organizationName_default	= World Wide Web Pty Ltd

organizationalUnitName		= Organizational Unit Name (eg, section)
#organizationalUnitName_default	=

commonName			= Common Name (e.g. server FQDN or YOUR name)
commonName_max			= 64

emailAddress			= Email Address
emailAddress_max		= 64
```

#### 3.4 - Você também precisa de alguns arquivos para rastrear os certificados assinados

| ***Comandos***

```bash
sudo echo "1001" > /etc/ssl/openldap/serial
```

```bash
sudo touch /etc/ssl/openldap/index.txt
```

#### 4 - Crie um arquivo CA Key executando o comando abaixo. Quando solicitado, insira a frase-senha

| ***Comando***

```bash
sudo openssl genrsa -aes256 \
	-out /etc/ssl/openldap/private/cakey.pem \
	4096
```

#### 4.1 - Para remover a frase-senha da chave CA

| ***Comando***

```bash
sudo openssl rsa \
	-in /etc/ssl/openldap/private/cakey.pem \
	-out /etc/ssl/openldap/private/cakey.pem
```

#### 4.2 - Crie o certificado CA. Certifique-se de definir o comum para corresponder ao FQDN do seu servidor (conforme editado no arquivo `/etc/lib/ssl/opensll.cnf`)

| ***Comando***

```bash
sudo openssl req -new -x509 \
	-days 3650 \
	-key /etc/ssl/openldap/private/cakey.pem \
	-out /etc/ssl/openldap/certs/cacert.pem
```

#### 5.0 - Especifique as extensões na configuração `openssl.cnf`, descomente a linha `req_extensions = v3_req # The extensions to add to a certificate request`

| ***Comando***

```bash
sudo vim /usr/lib/ssl/openssl.cnf
```
```bash
####################################################################
[ req ]
default_bits            = 2048
default_keyfile         = privkey.pem
distinguished_name      = req_distinguished_name
attributes              = req_attributes
x509_extensions = v3_ca # The extensions to add to the self signed cert

# Passwords for private keys if not present they will be prompted for
# input_password = secret
# output_password = secret

# This sets a mask for permitted string types. There are several options.
# default: PrintableString, T61String, BMPString.
# pkix   : PrintableString, BMPString (PKIX recommendation before 2004)
# utf8only: only UTF8Strings (PKIX recommendation after 2004).
# nombstr : PrintableString, T61String (no BMPStrings or UTF8Strings).
# MASK:XXXX a literal mask value.
# WARNING: ancient versions of Netscape crash on BMPStrings or UTF8Strings.
string_mask = utf8only

req_extensions = v3_req # The extensions to add to a certificate request
 ```


#### 5.1 - Adicionar os campos para solicitação de certificado na seção `v3_req`

```bash
[ v3_req ]

# Extensions to add to a certificate request

basicConstraints = CA:FALSE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = *.dcomp.local
DNS.2 = dcomp.local
```

#### 6.0 - Em seguida, gere a chave do servidor LDAP

| ***Comando***

```bash
sudo openssl genrsa -aes256 -out /etc/ssl/openldap/private/ldapserver-key.key 4096
```

#### 6.1 - Em seguida, gere a chave do servidor LDAP

| ***Comando***

```bash
sudo openssl rsa \
	-in /etc/ssl/openldap/private/ldapserver-key.key \
	-out /etc/ssl/openldap/private/ldapserver-key.key
```

#### 6.2 - Gere a solicitação de assinatura de certificado (CSR), dados conforme edição do arquivo de configuração `/etc/lib/ssl/openssl.cnf`

| ***Comando***

```bash
sudo openssl req -new \
	-key /etc/ssl/openldap/private/ldapserver-key.key \
	-out /etc/ssl/openldap/certs/ldapserver-cert.csr
```

#### 6.4 - Gere o certificado do servidor LDAP e assine-o com a chave CA e o certificado gerados acima

| ***Comando***

```bash
sudo openssl ca -keyfile /etc/ssl/openldap/private/cakey.pem \
	-cert /etc/ssl/openldap/certs/cacert.pem \
	-in /etc/ssl/openldap/certs/ldapserver-cert.csr \
	-out /etc/ssl/openldap/certs/ldapserver-cert.crt
```

| ***saída do comando***

```bash
Using configuration from /usr/lib/ssl/openssl.cnf
Check that the request matches the signature
Signature ok
Certificate Details:
        Serial Number: 4097 (0x1001)
        Validity
            Not Before: Feb 24 20:54:50 2024 GMT
            Not After : Feb 23 20:54:50 2025 GMT
        Subject:
            countryName               = US
            stateOrProvinceName       = California
            organizationName          = Kifarunix Inc
            organizationalUnitName    = IT Infrastructure
            commonName                = ldap.kifarunix.com
            emailAddress              = admin@kifarunix.com
        X509v3 extensions:
            X509v3 Basic Constraints: 
                CA:FALSE
            X509v3 Subject Key Identifier: 
                41:68:00:99:97:5D:83:D8:E3:88:2C:43:9D:5B:0A:B7:33:2A:F0:44
            X509v3 Authority Key Identifier: 
                BC:3C:1E:3C:D2:C4:BA:FD:5A:DB:AD:9B:90:A8:BD:57:4D:85:C1:B9
Certificate is to be certified until Feb 23 20:54:50 2025 GMT (365 days)
Sign the certificate? [y/n]:y


1 out of 1 certificate requests certified, commit? [y/n]y
Write out database with 1 new entries
Database updated
```

#### 7.0 - Verificar o servidor LDAP em relação à CA


| ***Comando***

```bash
sudo openssl verify -CAfile /etc/ssl/openldap/certs/cacert.pem /etc/ssl/openldap/certs/ldapserver-cert.crt
```


| ***Saída do comando***

```bash
/etc/ssl/openldap/certs/ldapserver-cert.crt: OK
```

 ***Agora, temos:*** 

- arquivo de certificado CA:`/etc/ssl/openldap/certs/cacert.pem`
* certificado do servidor: `/etc/ssl/openldap/certs/ldapserver-cert.crt`
+ arquivo de chave do servidor: `/etc/ssl/openldap/private/ldapserver-key.key`

#### 8.0 - Em seguida, defina a propriedade do diretório de certificados OpenLDAP para usuário *openldap*

| ***Comando***

```bash
sudo chown -R openldap: /etc/ssl/openldap/
```

#### 9.0 - Atualizar certificados TLS do servidor OpenLDAP

Em seguida, você precisa atualizar os certificados TLS do OpenLDAP Server. Portanto, crie um arquivo LDIF para definir os atributos TLS conforme mostrado abaixo.

| ***Comando***

```bash
sudo vim ldap-tls.ldif
```

| ***Editar = Substitua os locais dos seus certificados e arquivos de chave adequadamente***
```bash
dn: cn=config
changetype: modify
add: olcTLSCACertificateFile
olcTLSCACertificateFile: /etc/ssl/openldap/certs/cacert.pem
-
replace: olcTLSCertificateFile
olcTLSCertificateFile: /etc/ssl/openldap/certs/ldapserver-cert.crt
-
replace: olcTLSCertificateKeyFile
olcTLSCertificateKeyFile: /etc/ssl/openldap/private/ldapserver-key.key
```

#### 10 - Para atualizar o banco de dados LDAP, use  *ldapmodify* o comando conforme mostrado abaixo:

| ***Comando***

```bash
sudo ldapmodify -Y EXTERNAL -H ldapi:/// -f ldap-tls.ldif
```

| ***Saída do comando***

```bash
SASL/EXTERNAL authentication started
SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
SASL SSF: 0
modifying entry "cn=config"
```

#### 10.1 - Para verificar se os arquivos estão no lugar

| ***Comando***

```bash
sudo slapcat -b "cn=config" | grep -E "olcTLS"
```

| ***Saída do comando***

```bash
olcTLSCACertificateFile: /etc/ssl/openldap/certs/cacert.pem
olcTLSCertificateFile: /etc/ssl/openldap/certs/ldapserver-cert.crt
olcTLSCertificateKeyFile: /etc/ssl/openldap/private/ldapserver-key.key
```

#### 10.2 - Para verificar a validade da configuração LDAP, execute o comando abaixo

| ***Comando***

```bash
sudo slaptest -u
```

| ***Saída do comando***

```bash
config file testing succeeded
```

#### 10.3 - Em seguida, abra o  `/etc/ldap/ldap.conf` arquivo de configuração e altere o local do certificado da CA.

| ***Comando***

```bash
sudo vim /etc/ldap/ldap.conf
```

| ***Editar conforme***

```bash
...
# TLS certificates (needed for GnuTLS)
#TLS_CACERT	/etc/ssl/certs/ca-certificates.crt
TLS_CACERT	/etc/ssl/openldap/certs/cacert.pem
```

#### 10.4 - Reinicie o daemon OpenLDAP


| ***Comando***

```bash
sudo systemctl restart slapd
```

#### 11 - Para verificar a conectividade OpenLDAP TLS, execute o comando abaixo. Se a conexão estiver boa, você deve obter a saída,  `anonymous`.


| ***Comando***

```bash
ldapwhoami -H ldap://ldap.kifarunix.com -x -ZZ
```

| ***Comando***

```bash
ldapwhoami -H ldap://ldap.kifarunix.com -x -ZZ
```
| ***A saída dos comandos deve ser `anonymous`***

```bash
anonymous
```
