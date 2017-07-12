## Manifestação e Importação de XML (teknisa)

_Olá, gostaria de compartilhar uma forma simples que achei para que o processo de baixa de XMLs seja tranquilo. Esse tutorial precisa ser realizado por pessoas com algum conhecimento técnico ou  suporte da própria teknisa. Essa é uma forma que encontrei de retribuir o conhecimento e dedicação que a Teknisa tem comigo.__

A rotina de Manifestação e Importação de XML deixam a vida mais simples para dar entradas de notas fiscais. Quando essa rotina é feita de forma automática fica ainda mais prático. Recentemente solicitei para que fosse habilitada essa rotina no meu servidor e fizeram a instalação padrão:

- _XAMPP_
- _script php_
- JENKINS
- expor a porta 211 no servidor pelo bordland socket server

_os items em itálico não serão mais necessários_

### Problemas dessa abordagem com XAMPP

o XAMPP é instalado para somente se usar o PHP que vem no pacote, porém ele traz junto muitas coisas desnecessárias, como (Apache 2.4.26, MariaDB 10.1.25, PHP 5.6.31, phpMyAdmin 4.7.0, OpenSSL 1.0.2, XAMPP Control Panel 3.2.2, Webalizer 2.23-04, Mercury Mail Transport System 4.63, FileZilla FTP Server 0.9.41, Tomcat 7.0.56 (with mod_proxy_ajp as connector), Strawberry Perl 7.0.56 Portable) - uma solução para isso seria instalar o PHP da Microsoft mesmo, com o Microsoft Web Platform Installer 5.0
o script em PHP faz uma chamada COM+ da DLL da teknisa no servidor. Quando ocorrem erros no PHP eles ficam logados num arquivo e não são exibidos diretamente na tela. Isso deixa o processo um pouco obscuro para quem não conhece php.


### Um abordagem mais simples

Chamadas COM+ no windows podem ser feitas via powershell, nativo do próprio windows, de forma muito mais simples. Uma linha de comando resolve toda a questão.
O uso do Jenkins continua sendo um ótima abordagem para agendar estas chamadas do powershell


## Instalação e Configuração

Antes de tudo faça o teste na mão. Na sua linha de comando rode o comando abaixo.

```
powershell.exe -Command "(New-Object -ComObject DFE51000.rdmImpConsXML).ImportacaoXML('','')"
```

_Se a linha acima rodar rápido e responder 2 ou 3 significa que o seu servidor vai funcionar corretamente. Caso demore a retornar (uns 4min) e retorne um erro pode ser que tenha que configurar a porta 211:_

- Exponha a porta 211 pelo C:\Windows\SysWOW64\ScktSrvr.exe
- Instale a versão mais recente o Jenkins e realize as configurações iniciais (se deseja acessar o jenkins de outras máquinas, exponha a porta 8080 no seu firewall do roteador - ou configuração do provedor da nuvem - e deixe configurado um usuário com senha forte)
- Baixe o arquivo anexado e descompacte a pasta dentro de jobs do Jenkins (C:\Program Files (x86)\Jenkins\jobs)
- Em configurações do Jenkins use a opção "Recarregar configuração do disco"
- Teste o recém criado job clicando em "Construir agora" para este Job

Se tudo der certo você deverá receber a seguinte resposta:

```
Started by timer
Building in workspace C:\Program Files (x86)\Jenkins\workspace\Manifestacao-Importacao
[Manifestacao-Importacao] $ cmd /c call C:\Windows\TEMP\jenkins2844971601044394836.bat

C:\Program Files (x86)\Jenkins\workspace\Manifestacao-Importacao>powershell.exe -Command "(New-Object -ComObject DFE59000.rdmSincronizaNFE).SincronizaAutomatica('')" 
Configura‡Æo de e-mail do emitente inv lida.

C:\Program Files (x86)\Jenkins\workspace\Manifestacao-Importacao>exit 0 
[Manifestacao-Importacao] $ cmd /c call C:\Windows\TEMP\jenkins6298446477288709057.bat

C:\Program Files (x86)\Jenkins\workspace\Manifestacao-Importacao>powershell.exe -Command "(New-Object -ComObject DFE51000.rdmImpConsXML).ImportacaoXML('','')" 
2

C:\Program Files (x86)\Jenkins\workspace\Manifestacao-Importacao>exit 0 
Finished: SUCCESS
```
