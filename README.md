
<h2> Instalação zabbix-agent no Solaris 11.04 SUNOS 5.11</h2>

### Instalação do zabbix-agent

Este tutorial é para solaris (agora conhecido como Oracle Solaris) está baseado na versão 6.0 do zabbix-server e zabbix-agent versão 1 com Solaris na arquitetura amd64/i386, mas servirá para sparc também. Observação para a falta de linguagem GO no Solaris na arquitetura Sparc, existem forma de porte, mas não eh um processo oficial, esse suporte necessário para compilação do Zabbix-Agent2

Faça o download do arquivo .p5p disponível diretamente no site oficial do zabbix, link: [Zabbix_Agent_Solaris](https://www.zabbix.com/download_agents?version=6.0+LTS&release=6.0.14&os=Solaris&os_version=11&hardware=i386&encryption=OpenSSL&packaging=P5P&show_legacy=0)

>Esse .p5p é uma espécie de arquivo de contêiner que pode incluir vários pacotes de software, metadados e outros recursos necessários para a instalação e gerenciamento de software no Solaris. Esses pacotes podem conter aplicativos, bibliotecas, drivers, patches e outros componentes de software.

1. Crie uma pasta install no /tmp/

```bash
 mkdir /tmp/install
```
2. Faça a publicação no sistema do .p5p
```bash
pkg set-publisher -G '*' -g /tmp/install/zabbix_agent-6.0.14-solaris-11-i386-openssl.p5p Zabbix
```

3. Instalação do pacote publicado
```bash
$ pkg install zabbix-agent
```



### Estrutura de diretórios para arquivos do zabbix-agent e seu init file.

* Arquivos do binário do agentd
```
/opt/
	└── zabbix-agent 
	    ├── bin
	    │   ├── zabbix_get
	    │   └── zabbix_sender
	    ├── doc
	    │   └── LICENSE
	    ├── man
	    │   ├── man1
	    │   │   ├── zabbix_get.1
	    │   │   └── zabbix_sender.1
	    │   └── man8
	    │       └── zabbix_agentd.8
	    ├── sbin
	    │   └── zabbix_agentd
	    └── scripts
```

* Arquivos de configuração do Agent

```
/etc/opt/
	├── ssm
	│   └── oracle_hmp.conf
	└── zabbix-agent
	    ├── zabbix_agentd.conf
	    └── zabbix_agentd.conf.d
```

* Arquivo init file.

	/lib/svc/manifest/system/zabbix-agent.xml


### Gerenciar o serviço no Solaris

Para gerenciar serviços no Solaris, você pode usar o comando `svcs` para listar serviços, `svcadm` para habilitar, desabilitar e manipular serviços, e `init` ou `systemctl` para iniciar e parar serviços. Aqui estão alguns comandos úteis para gerenciar serviços no Solaris:

1. **Listar Serviços**:
   - `svcs`: Lista todos os serviços e seu status atual.
   - `svcs -a`: Lista todos os serviços, incluindo aqueles com falhas.
   - `svcs -x`: Lista serviços com falhas e informações detalhadas sobre as falhas.

2. **Habilitar/Desabilitar Serviços**:
   - `svcadm enable servicename`: Habilita um serviço.
   - `svcadm disable servicename`: Desabilita um serviço.
   
3. **Iniciar/Parar Serviços**:
   - `svcadm restart servicename`: Reinicia um serviço.
   - `svcadm refresh servicename`: Atualiza a configuração de um serviço.
   - `svcadm clear servicename`: Limpa erros persistentes em um serviço.
  
4. **Visualizar Informações Detalhadas do Serviço**:
   - `svcs -l servicename`: Mostra informações detalhadas sobre um serviço específico.

5. **Configuração de Serviços**:
   - Os arquivos de configuração de serviços estão geralmente localizados em `/lib/svc/manifest/` ou `/var/svc/manifest/`. Você pode editar esses arquivos para ajustar a configuração de um serviço.

6. **Verificar Status Geral do Sistema**:
   - `svcs -a`: Exibe o status de todos os serviços no sistema.
   - `svcs -xv`: Exibe informações detalhadas sobre serviços com falhas.

7. **Logs de Serviço**:
   - Os logs de serviços são armazenados em `/var/svc/log/` e podem ser úteis para solucionar problemas relacionados a serviços específicos.

8. **Inicialização Personalizada**:
   - O Solaris usa o SMF (Service Management Facility) para gerenciar serviços durante a inicialização. Você pode personalizar a inicialização de serviços usando o SMF.

Lembre-se de que o Solaris 11 usa o Service Management Facility (SMF) para gerenciar serviços, que é diferente do openrc ou do systemd em outros sistemas Unix/Linux. Portanto, os comandos e procedimentos de gerenciamento de serviços no Solaris podem ser diferentes dos distribuições mais comuns.
