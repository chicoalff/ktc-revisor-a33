Crie um script em PHP que execute as seguintes ações:

Copie todos os arquivos em formato json yaml yml PDF e md existentes nas seguintes pastas do repositório no github 

https://github.com/chicoalff/ktc-revisor-a33/tree/main/agent

https://github.com/chicoalff/ktc-revisor-a33/tree/main/workflow%2Fdiagnostic

https://github.com/chicoalff/ktc-revisor-a33/tree/main/knowledge

Salve todos os arquivos no servidor dentro da mesma pasta 
/public_html/revisor/

Caso algum arquivo já existe, crie uma pasta chamada old, renomeie o arquivo original incluído um timestamp no final e mova ele para a pasta old, então salve o novo arquivo 