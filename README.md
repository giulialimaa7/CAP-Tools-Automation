# CAP Tools Automation

Conjunto de extensões para Google Chrome desenvolvidas para automatizar tarefas repetitivas na análise de projetos de geração distribuída (energia solar fotovoltaica).

---

## Sobre o projeto

O CAP Tools Automation reúne três extensões independentes que eliminam o trabalho manual de copiar dados entre sistemas, preencher formulários e localizar coordenadas geográficas durante a análise de projetos.

As extensões coletam os dados de uma solicitação no portal da distribuidora, geram um arquivo XML, preenchem automaticamente os campos do sistema CAP (Cadastro e Análise de Projetos) a partir desse arquivo e agilizam a conferência da localização no Google Maps.

Todo o processamento acontece localmente no navegador, sem backend e sem dependências externas.

---

# Funcionalidades

## GENERATE-XML
- Botão "GERAR-XML" na página de detalhes da solicitação
- Coleta dos dados do titular, da unidade consumidora, dos módulos e do inversor
- Suporte a CPF ou CNPJ e escolha do melhor e-mail disponível
- Validação antes da geração do arquivo
- Download automático do XML
- Indicador de sessão ativa

## CAP-TOOLS
- Leitura do XML e preenchimento automático dos campos do CAP
- Padronização de tensão e subgrupo
- Cálculo da quantidade de módulos e da área
- Aviso visual de rateio ("COM RATEIO", "PRÉVIA S/ RATEIO", "NÃO TEM RATEIO")
- Botão MAPS para abrir a localização no Google Maps
- Botão EXTRACT para extrair coordenadas de um texto colado (graus/minutos/segundos ou decimal)
- Botão GO CAP para preencher latitude e longitude no formulário
- Botão SEARCH para consultar o inversor no site de consulta que desenvolvi (lê os dados de uma planilha do Google Sheets via API)
- Indicador de sessão ativa

## MAPS-AUTO-COPY
- Botão para copiar as coordenadas decimais do Google Maps
- Cópia automática ao carregar a página
- Contagem regressiva para fechar a aba, com opção de estender o tempo

---

# Tecnologias utilizadas

## Extensões
- JavaScript (vanilla)
- Chrome Extensions (Manifest V3)
- Content Scripts e Service Worker
- Chrome Scripting API

## Recursos do navegador
- DOM API
- DOMParser e FileReader
- Clipboard API
- Blob e download de arquivos
- Expressões regulares
- CSS

---

# Estrutura do projeto

```bash
CAP-Tools-Automation/
├── CAP-TOOLS/
│   ├── background.js
│   ├── content.js
│   ├── manifest.json
│   └── styles.css
├── GENERATE-XML/
│   ├── background.js
│   ├── content.js
│   ├── manifest.json
│   └── styles.css
└── MAPS-AUTO-COPY/
    ├── content.js
    ├── manifest.json
    └── styles.css
```

---

# Instalação

## Clone o repositório

```bash
git clone <URL_DO_REPOSITORIO>
```

---

## Carregue as extensões no Chrome

Não há dependências para instalar nem etapa de build. Cada pasta é uma extensão independente.

1. Acesse `chrome://extensions`
2. Ative o **Modo do desenvolvedor**
3. Clique em **Carregar sem compactação**
4. Selecione a pasta da extensão (`CAP-TOOLS`, `GENERATE-XML` ou `MAPS-AUTO-COPY`)
5. Repita para as demais extensões

> Após alterar qualquer arquivo, clique em **Recarregar** na extensão e atualize a página.

---

# Fluxo de uso

1. Na página de detalhes da solicitação, o usuário clica em **GERAR-XML** e o arquivo XML é baixado
2. No CAP, o usuário seleciona o XML e os campos do formulário são preenchidos
3. O indicador de rateio é exibido no canto da tela
4. O botão **MAPS** abre a localização no Google Maps, onde a **MAPS-AUTO-COPY** copia as coordenadas
5. Em **EXTRACT**, o usuário cola o texto com as coordenadas e clica em **EXTRAIR**
6. O botão **GO CAP** preenche latitude e longitude no formulário
7. O botão **SEARCH** consulta o fabricante do inversor informado

---

# Privacidade e segurança

- Os dados são processados localmente no navegador e nenhuma informação é armazenada ou enviada para servidor próprio
- Os botões MAPS e SEARCH abrem sites externos e enviam, na URL, as coordenadas e o texto do campo do inversor
- Os arquivos XML gerados contêm dados pessoais (CPF/CNPJ, e-mail, telefone e endereço). Não os versione nem os compartilhe
- Utilize as extensões apenas em sistemas e com dados que você tem autorização para acessar

---

# Diferenciais do projeto

- Automação de uma rotina manual e repetitiva
- Três extensões independentes e de instalação simples
- JavaScript puro, sem dependências externas
- Padronização e validação dos dados antes do preenchimento
- Reconhecimento de coordenadas em mais de um formato
- Projeto voltado a uma rotina real de análise de projetos

---

# Melhorias futuras

- Restringir as URLs (`matches`) da CAP-TOOLS ao endereço do sistema CAP (hoje a extensão roda em qualquer página e só age quando o título corresponde)
- Tornar os seletores e as expressões regulares mais resistentes a mudanças no layout dos sistemas
- Configuração dos tempos de sessão e de contagem por uma tela da extensão
- Unificar as extensões em um único pacote
- Testes automatizados

---

# Autor

Desenvolvido por Giulia Vargas.
