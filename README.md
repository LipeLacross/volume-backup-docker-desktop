## 🌐 [English Version of README](README_EN.md)

# Docker Volume Backup and Restore

Este é um utilitário para realizar backup e restauração de volumes Docker. Funciona da mesma forma com volumes do Podman, basta substituir o comando `docker` por `podman`.

> **Aviso**: Certifique-se de que nenhum contêiner esteja utilizando o volume antes de realizar o backup ou a restauração, caso contrário, seus dados podem ser corrompidos. Consulte "Miscellaneous" para mais informações.

> **Nota**: Quando estiver usando `docker-compose`, não se esqueça de fazer backup e restaurar as labels dos volumes. Consulte "Miscellaneous" para mais detalhes.

## 🔨 Funcionalidades do Projeto

- Realiza backup de volumes Docker com suporte para diferentes algoritmos de compressão.
- Restaura volumes a partir de arquivos de backup, com a possibilidade de forçar a substituição de dados.
- Suporte para exclusão de arquivos ou diretórios durante o backup.
- Permite a migração de volumes entre diferentes hosts via SSH.
- Opção para preservar labels de volumes quando necessário (importante para `docker-compose`).

### Exemplo Visual do Projeto

**Backup de volume:**

```bash
docker run -v some_volume:/volume --rm --log-driver none loomchild/volume-backup backup > some_archive.tar.bz2
```

**Restauração de volume:**

```bash
docker run -i -v some_volume:/volume --rm loomchild/volume-backup restore < some_archive.tar.bz2
```

## ✔️ Técnicas e Tecnologias Utilizadas

- **Docker**: Para criar, gerenciar e manipular volumes.
- **Tar**: Utilizado para arquivar e extrair dados durante o processo de backup e restauração.
- **Algoritmos de Compressão**: Suporte para `bz2`, `gz`, `xz`, `pigz`, `zstd` e sem compressão.

## 📁 Estrutura do Projeto

- **Dockerfile**: Arquivo para construir a imagem Docker.
- **LICENSE**: Licença do projeto.
- **README.md**: Documentação do projeto.
- **volume-backup.sh**: Script principal para backup e restauração de volumes Docker.

## 🛠️ Abrir e rodar o projeto

Para usar este utilitário localmente, siga os passos abaixo:

1. **Certifique-se de que o Docker está instalado**:
   - O [Docker](https://www.docker.com/get-started) é necessário para rodar o projeto. Você pode verificar se já o tem instalado com:

     ```bash
     docker -v
     ```

   - Se não estiver instalado, baixe e instale a versão recomendada.

2. **Clone o Repositório**:
   - Copie a URL do repositório e execute o comando abaixo no terminal:

     ```bash
     git clone https://github.com/usuario/volume-backup.git
     cd volume-backup
     ```

3. **Construir a Imagem Docker**:
   - Execute o comando abaixo para construir a imagem Docker:

     ```bash
     docker build -t volume-backup .
     ```

4. **Rodar o Backup ou Restauração**:
   - **Backup**:

     ```bash
     docker run -v [volume-name]:/volume --rm --log-driver none volume-backup backup > [archive-path]
     ```

   - **Restauração**:

     ```bash
     docker run -i -v [volume-name]:/volume --rm --log-driver none volume-backup restore < [archive-path]
     ```

## 🌐 Deploy

Para fazer o deploy em produção ou em outro ambiente, siga as instruções abaixo:

1. **Subir a imagem para o DockerHub ou GitHub Container Registry**:
   - Você pode fazer o push da imagem para o DockerHub ou usar o GitHub Container Registry para evitar limitações de uso no DockerHub.

     ```bash
     docker tag volume-backup ghcr.io/username/volume-backup
     docker push ghcr.io/username/volume-backup
     ```

2. **Rodar o Backup ou Restauração no Host de Produção**:
   - No host de produção, execute os mesmos comandos de backup e restauração descritos acima.

## Miscellaneous

- **Atualizar a imagem**:

  ```bash
  docker pull loomchild/volume-backup
  ```

  Ou use o GitHub Container Registry para evitar limitações no DockerHub:

  ```bash
  docker pull ghcr.io/loomchild/volume-backup
  ```

- **Encontrar contêineres utilizando um volume**:

  ```bash
  docker ps -a --filter volume=[volume-name]
  ```

- **Excluir arquivos do backup**:

  ```bash
  docker run -v [volume-name]:/volume --rm --log-driver none loomchild/volume-backup backup -e [excluded-glob] > [archive-path]
  ```

- **Usar algoritmo de compressão diferente**:

  ```bash
  docker run -v [volume-name]:/volume --rm --log-driver none loomchild/volume-backup backup -c pigz > [archive-path]
  ```

- **Exibir indicador de progresso**:

  ```bash
  docker run -v [volume-name]:/volume --rm --log-driver none loomchild/volume-backup backup -v > [archive-path]
  ```

- **Passar argumentos adicionais para o `tar`**:

  ```bash
  docker run -v [volume-name]:/volume --rm --log-driver none loomchild/volume-backup backup -x --verbose > [archive-path]
  ```

- **Migrar volume diretamente para um novo host via SSH**:

  ```bash
  docker run -v [volume-name]:/volume --rm --log-driver none loomchild/volume-backup backup | ssh [receiver] docker run -i -v [volume-name]:/volume --rm loomchild/volume-backup restore
  ```

- **Preservar labels de volumes**: Caso você utilize o `docker-compose`, salve e restaure as labels com:

  ```bash
  docker inspect [volume-name] -f "{{json .Labels}}" > labels.json
  ```

  Durante a restauração, crie o volume manualmente com as labels:

  ```bash
  docker volume create --label "label1" --label "label2" [volume-name]
  ```

```
