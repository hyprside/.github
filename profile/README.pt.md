# Hyprside

Windows 10 está quase a acabar, windows 11 é um desastre, distribuições linux não são uma opção pra maioria das pessoas e MacOS é muito caro. Sistemas operativos para telemóveis como Android e iOS evoluiram muito em usabilidade, integração e estabilidade, mas sistemas operativos para computadores estagnaram.

**Hyprside** é um sistema operativo baseado em Linux, pensado para ser o verdadeiro sucessor das distribuições tradicionais e até do Windows.
O foco é oferecer **estabilidade, desempenho e uma experiência de utilizador coesa e elegante**, sem a fragmentação e fragilidade comuns nos sistemas atuais.

> O nome *Hyprside* vem da ideia de ser *“o outro lado.”*

**AVISO: O projeto ainda está em desenvolvimento. Este README não descreve o estado atual do projeto, mas sim o que se pretende criar.**

---

## ✨ Objetivos Principais

* UX/UI ao nível da Apple — a melhor experiência visual e de design no ecossistema Linux.
* Experiência consistente e uniforme em todo o sistema.
* Design moderno, animações suaves e transições bem integradas.
* Interface intuitiva e livre de bugs.
* Estavel: não queremos funcionalidades que só funcionam às vezes, se funciona uma vez, funciona sempre.
* **Consistência visual e comportamental** entre todas as aplicações, independentemente do toolkit.
* **GUI-first**: todas as operações essenciais podem ser feitas sem terminal (o terminal existe apenas como ferramenta de desenvolvimento e vem desativado por defeito).
* Integração transparente com aplicações Windows e Android.
* Snapshots e rollback via Btrfs sem necessidade de ferramentas externas.

**Hyprside** não é apenas “mais uma distribuição Linux”, mas sim um **sistema independente**, semelhante ao Android ou ChromeOS.

---

## 🖥️ Componentes Principais

### 1. tibs — Tiago’s Incredible Boot Screen (Gestor de Boot e Display)

* Standalone, sem Wayland/X11.
* Stack técnico: **Clay + Skia + OpenGL** (mesmo do HyprUI).
* Gere o login e o ecrã de bloqueio.
* **Substitui completamente o Plymouth**: gere a splash screen e a experiência de boot.
* Monitoriza o progresso do boot através dos **serviços do systemd**.
* Já está implementado em grande parte, apenas precisa de alguns ajustes.

### 2. HyprDE (Ambiente de Trabalho)

* Baseado em **Hyprland**, totalmente integrado com HyprUI e temas universais.
* UX/UI ao nível da Apple, com transições suaves e usabilidade refinada.
* **Subsistema Windows**: integração Wine/Proton, rollback via snapshots Btrfs, suporte para apps problemáticas (Adobe Suite, MS Office).
* **Subsistema Android**: integração transparente com WayDroid.
* **Notificações avançadas**: estilo Android, interativas e persistentes.
* **Clipboard inteligente**: histórico, edição e partilha.
* **Drawers unificados**: sudo, polkit, file picker, screen share, etc.
* **File picker universal**: funciona em todos os toolkits.

### 3. Loja de Aplicações

* Backend próprio (sem PackageKit), rápido e fiável.
* Integração com [Flatpak](https://flatpak.org/) e um gestor de pacotes no modo desenvolvedor
  * O gestor de pacote ainda precisa de ser pensado e planeado
* Integração com o subsistema Windows, permitindo instalar aplicações Windows na mesma loja.

### 4. Aplicação de Configurações

* Gestão completa do sistema via GUI (substitui terminal).
* Gestão multi-GPU, permissões, temas, animações, gravação de ecrã, etc.
* Exportar/importar configurações para replicar setups em outros dispositivos.
* **Hyprtheme**: ficheiro TOML único, inspirado no Tailwind, aplicado a GTK, Qt, HyprUI, etc.

---

## 🎨 Arquitetura de UI / Toolkits

### HyprUI

* Toolkit próprio escrito em **Rust**.
* Baseado em **Clay + Skia**, com API declarativa inspirada no React.
* API unificada para aplicações nativas e shells Wayland.

### Hyprtheme

* Formato TOML universal para temas.
* Resolve a fragmentação GTK/Qt.
* Garante consistência visual e comportamental em todo o sistema.

---

## 🔀 HyprSessionManager (HSM)

* Compositores (Hyprland, tibs) renderizam para **framebuffers off-screen** e enviam para o HSM.
* O HSM decide qual buffer aparece em cada ecrã, aplica transformações e faz a composição final para DRM/KMS.
* Controla quem recebe inputs de rato e teclado
* Gere atalhos privilegiados (Win+L, Ctrl+Alt+Del).
* **Segurança**: a troca de sessões é controlada pelo HSM, evitando bypass via TTY.
* Faz transições suaves entre telas
```
tibs           (🔒) ┅┅┅> session manager ━> DRM (screen)
user1 hyprland (✅) ━━━━━━━━━┫
user2 hyprland (🔒) ┅┅┅┅┅┅┅┅┅┛
```

---

## 📋 Funcionalidades Adicionais

* Gravação de ecrã via GPU integrada no shell.
* Debounce para pedidos repetidos de permissões.
* File picker unificado (substitui os nativos GTK/Qt).

---

## 🔐 Filosofia

* Simples, elegante, coeso e performante.
* Consistência total entre apps, UI e comportamento do sistema.
* GUI-first: o terminal nunca é obrigatório.
* Segurança e design como pilares centrais.

---

## 📦 Arquitetura do Sistema

Hyprside baseia-se no conceito de **ROM imutável**, inspirado no Android e ChromeOS:

* O sistema é distribuído como uma **imagem read-only (`.img`)**.
* Apenas `/home` e alguns diretórios específicos são mutáveis.
* Atualizações atómicas: descarregar nova imagem → substituir → reiniciar.
* Sem risco de corromper o sistema, como acontece com `apt upgrade` ou `pacman -Syu`.

### Gestão de Software

* Apps via **Flatpak**, com menos restrições.
* Modo desenvolvedor permite utilizar gestor de pacotes completo.
* Possibilidade de imagens customizadas (self-hosting ou builds próprios).

### Perfis de Utilizador

* **Utilizador normal:** acesso apenas a `/home`.
* **Administrador:** apps e configurações globais.
* **root/SYSTEM:** reservado a operações críticas do sistema.

### Vantagens

* **Estabilidade:** a base nunca é corrompida.
* **Reprodutibilidade:** setups facilmente replicáveis entre máquinas.
* **Simplicidade:** formatar é trivial (basta limpar os subvolumes mutáveis).
* **Portabilidade:** arquitetura facilita portar o Hyprside para smartphones Android no futuro.

---

## 🚀 Estado Atual

**tibs** e **HyprUI** já estão em desenvolvimento.
Hyprside é um projeto em progresso com o objetivo a longo prazo de se tornar um sistema estável e independente, potencialmente **pré-instalado em hardware** no futuro.

---
