# 🛍️ CheckCommerce Test Shopify Liquid

<div align="center">

![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-yellow?style=for-the-badge&logo=javascript)
![HTML](https://img.shields.io/badge/HTML-5-orange?style=for-the-badge&logo=html5)
![CSS](https://img.shields.io/badge/CSS-3-blue?style=for-the-badge&logo=css3)
![Liquid](https://img.shields.io/badge/Liquid-Shopify-7952B3?style=for-the-badge&logo=shopify)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

**Customizações realizadas no tema Dawn para atender aos requisitos técnicos.** 🎉

_Uma aplicação JS + HTML + CSS + Liquid focada em arquitetura moderna, componentes reutilizáveis e experiência de usuário responsiva._

</div>

---

## 🗂️ Estrutura do Projeto
```
shopify-dawn-custom/
├── layout/
│   └── theme.liquid             # Ajustes de performance (preconnect)
├── sections/
│   ├── image-with-text.liquid   # Suporte a vídeo no lugar da imagem
│   ├── slideshow.liquid         # Suporte a imagens otimizadas para mobile
│   └── ...                      # Outras seções originais do tema
├── assets/
│   └── custom-scripts.js        # Recomendação de centralização de JS
├── snippets/
│   └── ...                      # Componentes auxiliares
```

---

## 🧩 Relatório de Alterações Técnicas

Este documento detalha todas as mudanças realizadas no tema Dawn para atender aos requisitos do teste técnico, incluindo justificativas e benefícios de cada ajuste.

### 1️⃣ Seção **Imagem com texto** (`image-with-text.liquid`)

❓ **O que foi alterado:**
- Adicionado um campo do tipo `video` ao schema da seção, permitindo selecionar um vídeo hospedado no Shopify pelo editor visual.
- Alterado o template para exibir o vídeo (com autoplay, muted, loop, playsinline e lazy loading) caso o campo esteja preenchido. Caso contrário, mantém o comportamento padrão de exibir a imagem.

⚙️ **Como foi alterado:**
- No schema, incluído:
  ```json
  {
    "type": "video",
    "id": "video",
    "label": "Vídeo (substitui a imagem se selecionado)",
    "info": "Selecione um vídeo hospedado no Shopify para exibir no lugar da imagem."
  }
  ```
- No template, inserido bloco condicional para renderizar o vídeo com os atributos recomendados para performance e acessibilidade.

✅ **Benefícios:**
- Flexibilidade para o lojista exibir vídeos institucionais ou promocionais.
- Melhora a experiência visual e engajamento do usuário.
- Lazy loading reduz o impacto no carregamento inicial da página.

---

### 2️⃣ Seção **Apresentação de slides** (`slideshow.liquid`)

❓ **O que foi alterado:**
- Adicionado campo `image_mobile` ao schema do bloco de slide, permitindo selecionar uma imagem alternativa para dispositivos móveis.
- Alterado o template para exibir a imagem mobile apenas em telas pequenas (até 749px) e a imagem desktop em telas grandes (a partir de 750px), usando o elemento `<picture>` e media queries.
- Adicionado atributos `width`, `height` e `alt` para acessibilidade e performance.

⚙️ **Como foi alterado:**
- No schema, incluído:
  ```json
  {
    "type": "image_picker",
    "id": "image_mobile",
    "label": "Imagem para dispositivos móveis"
  }
  ```
- No template, substituído o bloco de imagem por:
  ```liquid
  <picture>
    <source media="(max-width: 749px)" srcset="{{ block.settings.image_mobile | image_url: width: 750 }}">
    <source media="(min-width: 750px)" srcset="{{ block.settings.image | image_url: width: 1500, height: 750 }}">
    <img src="{{ block.settings.image_mobile | image_url: width: 750, height: 750 }}" width="750" height="750" alt="{{ block.settings.image_mobile.alt | escape }}" loading="lazy" style="display: block; width: 100%; height: auto;">
  </picture>
  ```

✅ **Benefícios:**
- Reduz o consumo de banda em dispositivos móveis.
- Garante que imagens sejam exibidas corretamente e com melhor qualidade em cada contexto.
- Lazy loading nas imagens mobile melhora o tempo de carregamento.

---

### 3️⃣ Performance Web
- Adicionado `loading="lazy"` em imagens do slideshow.
- Inclusão de `<link rel="preconnect">` para fontes externas em `theme.liquid`.
- Reforço sobre minificação e eliminação de recursos não utilizados.
- ✅ **Benefício**: Melhora métricas **LCP** e Web Vitals, reduzindo tempo de carregamento.

---

## ⚡ Melhorias Recomendadas

1. **Acessibilidade**
   - Garantir `alt` descritivo em todas imagens.
   - Adicionar legendas (`<track>`) em vídeos.
   - ✅ **Impacto**: Inclusão de usuários de leitores de tela e SEO.

2. **JavaScript Inline**
   - Substituir scripts inline (`<script>`) por arquivos externos em `assets/`.
   - ✅ **Impacto**: Melhor cache, manutenção e minificação.

3. **Responsividade**
   - Revisar seções como `collage.liquid`, `multicolumn.liquid`, `multirow.liquid`.
   - Uso consistente de classes utilitárias e media queries.
   - ✅ **Impacto**: Layout consistente em todos dispositivos.

---

## 🧪 Testes Automatizados

- **Unitários (JS):** Jest recomendado para lógica isolada.
- **Integração visual (Liquid):** Cypress ou Playwright.
- ✅ **Benefício**: Redução de regressões e maior confiabilidade em entregas.




**O que foi sugerido e aplicado:**
- Adicionado o atributo `loading="lazy"` nas imagens do slideshow para garantir que apenas imagens visíveis sejam carregadas inicialmente.
- Inclusão da tag `<link rel="preconnect">` para servidores de fontes externas no arquivo `layout/theme.liquid`, removendo a condição para garantir que o preconnect sempre seja utilizado, independentemente das configurações de fonte.
- Reforçada a importância da minificação de arquivos CSS/JS e eliminação de recursos não utilizados.

**Por que foi sugerido e realizado:**
- Lazy loading é uma boa prática recomendada pelo Google e pelo Web Vitals, reduzindo o tempo de carregamento e o consumo de banda.
- O preconnect sempre ativo garante que a conexão com servidores de fontes externas será otimizada em todos os cenários, acelerando o carregamento das fontes e evitando atrasos, mesmo que o lojista altere configurações de fonte no futuro.
- Minificação e eliminação de recursos não utilizados reduzem o peso da página e melhoram a performance geral.

**Benefícios:**
- Melhora o LCP (Largest Contentful Paint) e o tempo de resposta em dispositivos móveis e desktop.
- Reduz o consumo de recursos e melhora a experiência do usuário.
- Garante performance consistente, independentemente das configurações de fonte escolhidas pelo lojista.

### Pontos de melhoria identificados nas seções do tema:

1. **Acessibilidade em imagens e vídeos**
   - Diversos arquivos de seção que exibem imagens (ex: `image-banner.liquid`, `featured-product.liquid`, `slideshow.liquid`) podem não garantir que o atributo `alt` esteja sempre preenchido corretamente, o que prejudica usuários de leitores de tela e SEO. Além disso, vídeos podem não incluir legendas/captions, dificultando o acesso para pessoas com deficiência auditiva.
   - **Exemplo de problema:**
     ```liquid
     <img src="{{ section.settings.image | image_url }}">
     ```
     (Sem atributo `alt` ou com valor genérico)
   - **Exemplo de melhoria:**
     ```liquid
     <img src="{{ section.settings.image | image_url }}" alt="{{ section.settings.image.alt | escape }}">
     ```
     Para vídeos:
     ```liquid
     <video src="{{ section.settings.video | video_url }}" controls>
       <track kind="captions" src="{{ section.settings.captions | file_url }}" srclang="pt" label="Português">
     </video>
     ```
   - **Melhoria recomendada:** Validar que todas as imagens tenham atributo `alt` descritivo e que vídeos incluam legendas ou transcrições sempre que possível.

2. **Uso de JavaScript Inline**
   - Seções como `cart-drawer.liquid` e `cart-notification-product.liquid` podem conter scripts inline ou dependências JS diretamente no arquivo Liquid. Isso dificulta a minificação, o cache e a manutenção do código.
   - **Exemplo de problema:**
     ```liquid
     <script>
       document.getElementById('cart-drawer').addEventListener('click', function() { ... });
     </script>
     ```
   - **Exemplo de melhoria:**
     ```liquid
     <script src="{{ 'cart-drawer.js' | asset_url }}" defer></script>
     ```
     (Mover o código JS para um arquivo externo e referenciar via asset)
   - **Melhoria recomendada:** Centralizar scripts em arquivos JS externos e evitar código inline, facilitando otimização de performance e organização do projeto.

3. **Falta de Responsividade em Componentes Customizados**
   - Seções como `collage.liquid`, `multicolumn.liquid` e `multirow.liquid` podem não estar totalmente otimizadas para todos tamanhos de tela, especialmente em layouts mais complexos. Isso pode prejudicar a experiência do usuário em dispositivos móveis.
   - **Exemplo de problema:**
     ```liquid
     <div class="multirow">
       <!-- sem classes responsivas ou media queries -->
     </div>
     ```
   - **Exemplo de melhoria:**
     ```liquid
     <div class="multirow grid grid--responsive">
       <!-- aplicar classes CSS e media queries para responsividade -->
     </div>
     ```
     E no CSS:
     ```css
     @media (max-width: 749px) {
       .grid--responsive { flex-direction: column; }
     }
     @media (min-width: 750px) {
       .grid--responsive { flex-direction: row; }
     }
     ```
   - **Melhoria recomendada:** Revisar e garantir que todos componentes utilizem classes responsivas e media queries adequadas, realizando testes em dispositivos móveis e desktop para assegurar boa usabilidade.

---

## 4. Testes automatizados

**Recomendação:**
- Estruturar testes unitários para arquivos JavaScript do tema, utilizando frameworks como Jest.
- Para arquivos Liquid, recomenda-se testes de integração visual (ex: Cypress, Playwright).

**Benefícios:**
- Maior confiabilidade e facilidade de manutenção do tema.
- Redução de bugs e problemas em funcionalidades críticas.

**Adendo:**
Embora a realização de testes automatizados para templates e temas Shopify não seja uma prática universal entre todos desenvolvedores do mercado, ela é cada vez mais recomendada em projetos profissionais e por agências especializadas. O uso de testes (unitários para JS e integração visual para Liquid) garante maior qualidade, facilita atualizações e reduz riscos de regressão, sendo um diferencial competitivo para equipes que buscam excelência e confiabilidade em software frontend.

---

## Resumo
Essas alterações tornam o tema Dawn mais flexível, moderno e otimizado para diferentes dispositivos, além de seguir boas práticas de performance e acessibilidade recomendadas para temas Shopify.
