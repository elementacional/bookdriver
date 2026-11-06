# Book Driver v1.0 — Biblioteca Google Drive

Aplicativo independente, em HTML estático, para **catalogar arquivos PDF de pastas próprias e compartilhadas do Google Drive**, com tela dark, busca por título, exploração de subpastas e capas produzidas a partir da primeira página dos arquivos quando a leitura é autorizada.

## Instalação no GitHub Pages

1. Crie um repositório, por exemplo `bookdriver`, e publique `index.html` na raiz da branch `main`.
2. Em **Settings → Pages**, configure **Deploy from a branch → main → / (root)**.
3. Abra `https://SEU-USUARIO.github.io/bookdriver/`.
4. No Google Cloud Console, crie/selecione um projeto, habilite a **Google Drive API** e configure a **Tela de consentimento OAuth** (incluindo usuários de teste quando aplicável).
5. Crie um **OAuth Client ID — Aplicativo da Web** e adicione **`https://SEU-USUARIO.github.io`** em **Authorized JavaScript origins**. A origem inclui protocolo/domínio, sem o caminho `/bookdriver/`. Se usar domínio próprio, cadastre também essa origem.
6. No Book Driver, selecione **Configurar Google** e cole o **Client ID** (não um client secret). Clique em **Conectar Google** e autorize o acesso.
7. Selecione **Adicionar fontes**, explore Meu Drive ou Compartilhadas comigo, abra pastas, marque as desejadas e clique **Salvar e sincronizar**.

## Funcionalidades

- Login Google via **Google Identity Services (OAuth 2.0)**; sem senhas no HTML.
- Lista pastas do Meu Drive e pastas compartilhadas com a conta autenticada, quando expostas pela Drive API.
- Seleção de múltiplas pastas e varredura recursiva dos PDFs, inclusive em subpastas.
- Capas geradas da primeira página com PDF.js após obter o PDF via Drive API; quando bloqueado ou indisponível, exibe capa genérica.
- Pesquisa pelo nome do arquivo/pasta e abertura do PDF no visualizador Google Drive.
- Sincronização ao clicar no botão e a cada cinco minutos **com a aba aberta e a sessão autorizada**.
- Catálogo e fontes armazenados somente em `localStorage` no navegador. **Metadados de livros privados podem permanecer neste dispositivo**, inclusive após desconectar: use navegador/perfil confiável.

## Limitações importantes

- A aplicação **não conecta a conta Google automaticamente**: é necessário criar o próprio Client ID no Google Cloud. O escopo `drive.readonly` é **restrito**, e um aplicativo disponibilizado amplamente pode exigir verificação e requisitos adicionais do Google. Para um protótipo pessoal, a tela de consentimento pode usar usuários de teste permitidos e limites aplicáveis.
- A sincronização **não acontece com o navegador fechado**, não atualiza catálogos de outros usuários e não salva PDFs dentro do GitHub. Para sincronizar remotamente, é preciso backend/autenticação e um banco de dados.
- Uma pasta apenas compartilhada por link pode **não aparecer** em Compartilhadas comigo sem ter sido efetivamente disponibilizada à conta; a permissão do usuário e os atalhos do Drive também podem influenciar a descoberta.
- A interface lista pastas compartilhadas diretamente com você. Dentro delas, navega em subpastas. Não inclui um seletor especial de unidades compartilhadas (*Shared Drives*) ou resolução automática de atalhos do Drive nesta primeira versão.
- PDFs grandes podem consumir memória ao gerar capas; a Drive API tem limites de uso. Documentos bloqueados para cópia/download podem não renderizar a capa.
- A biblioteca PDF.js é carregada de um CDN externo e requer conexão à internet; o aplicativo também precisa de conexão para autenticar e consultar PDFs.
- Nenhum token de acesso é gravado no `localStorage`; o Client ID é público e pode ser salvo. Não armazene *client secret* no frontend.
- O comportamento da integração depende de políticas do Google, de CORS e das permissões dos arquivos.

## Privacidade

Esta é uma biblioteca **pessoal autenticada**. **Publicar a interface no GitHub Pages não torna públicos os PDFs privados**. O acesso é feito com as permissões de cada conta Google. Nunca publique tokens, segredos nem URLs temporárias de download.

## Validação

O JavaScript foi verificado quanto à sintaxe, e o pacote ZIP foi checado. A autenticação OAuth e a navegação com arquivos reais dependem da configuração do Google Cloud e devem ser testadas no domínio definitivo.
