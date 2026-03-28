```markdown
# Diretrizes de Design: Alta Joalheria & Gestão Digital

## 1. Visão Geral e Estrela do Norte Criativa
Este sistema de design foi concebido sob o conceito da **"Curadoria Editorial"**. Diferente de painéis administrativos genéricos e saturados de linhas, este ecossistema visual deve ser tratado como uma vitrine de luxo. O objetivo é equilibrar a autoridade institucional do preto profundo (`#1A1A1A`) com a sofisticação tátil do ouro (`#C9A84C`).

**Estrela do Norte: Minimalismo de Prestígio.**
O design rompe com a estética de "template" ao priorizar o respiro (white space) e a profundidade tonal em vez de divisórias rígidas. A interface não deve parecer um software, mas sim um catálogo digital de alta gama, onde a precisão técnica do JetBrains Mono encontra a elegância funcional da Inter.

---

## 2. Cores e Texturas
A paleta é fundamentada no contraste entre a luz (Off-white/Pure White) e o mistério (Deep Black), com o ouro servindo como o condutor de ação e valor.

### A Regra "No-Line" (Sem Linhas)
Está terminantemente proibido o uso de bordas sólidas de 1px para separar seções. A arquitetura de informação deve ser definida exclusivamente por:
- **Transições Tonais:** Use a alternância entre `surface` (#FAFAFA) e `surface-container-low` (#F3F3F3).
- **Respiro Negativo:** Utilize a escala de espaçamento (ex: `spacing-8` ou `spacing-12`) para criar distanciamento rítmico.

### Hierarquia de Superfícies
Trate a UI como camadas físicas de papel premium e vidro:
- **Nível 0 (Base):** `surface` (#FAFAFA) para o background geral.
- **Nível 1 (Cartões):** `surface-container-lowest` (#FFFFFF) com cantos arredondados de 8px.
- **Nível 2 (Modais/Popovers):** `surface-container-lowest` com efeito de "Glassmorphism" (backdrop-blur de 12px e opacidade de 95%) para sobreposição suave.

### Gradientes de Assinatura
Para CTAs principais e estados de destaque, não utilize cores chapadas. Aplique um gradiente sutil:
- **Gold Luxe:** Linear de `secondary` (#755B00) para `secondary-fixed-dim` (#E6C364) a 135 graus. Isso confere "alma" e volume metálico ao componente.

---

## 3. Tipografia: A Voz do Especialista
A tipografia é o elemento que comunica a precisão da joalheria.

*   **Display & Headlines (Inter):** Devem ser usadas com pesos `SemiBold` ou `Medium`. O tracking (espaçamento entre letras) deve ser levemente reduzido (-1% ou -2%) para títulos grandes, evocando uma estética de revista de moda.
*   **Body (Inter):** O corpo do texto utiliza `Regular` para leitura fluida, garantindo legibilidade em descrições de gemas e metais.
*   **Data & Codes (JetBrains Mono):** **Crucial.** Todo número de SKU, quilatagem, valores monetários e códigos de inventário devem usar JetBrains Mono. Isso separa visualmente o "conteúdo descritivo" dos "dados técnicos", conferindo um ar de oficina de precisão.

---

## 4. Elevação, Profundidade e "Ghost Borders"
A profundidade no sistema de luxo é sentida, não vista.

*   **O Princípio do Empilhamento:** Em vez de sombras pesadas, coloque um card `surface-container-lowest` sobre um fundo `surface-container-low`. O contraste de luminosidade criará a separação necessária.
*   **Sombras Ambientais:** Quando o flutuar for inevitável (ex: menus dropdown), use sombras com `blur: 32px`, `spread: -4px` e opacidade de 4% a 6% do tom `on-surface`. A sombra deve parecer luz natural difusa.
*   **Ghost Border Fallback:** Se uma borda for estritamente necessária (ex: inputs), utilize o token `outline-variant` com 20% de opacidade. Nunca use 100% de contraste em bordas.

---

## 5. Componentes Principais

### Botões (Ações de Prestígio)
*   **Primary:** Gradiente Gold Luxe, sem borda, texto em `on-secondary-fixed` (Preto profundo). Cantos de 6px.
*   **Secondary:** Fundo `surface-container-highest` com texto `primary`. Elegantemente discreto.
*   **Tertiary:** Apenas texto com JetBrains Mono para ações técnicas, sublinhado apenas no hover.

### Campos de Entrada (Inputs)
*   Cantos de 6px. Fundo `surface-container-lowest`.
*   **Estado de Foco:** Não use bordas azuis padrão. Utilize um anel de foco (ring) de 2px em `secondary` (#C9A84C) com um offset de 2px para criar um "halo" dourado sofisticado.

### Tabelas de Inventário
*   **Regra de Ouro:** Proibido o uso de linhas divisórias horizontais.
*   **Alternativa:** Use o estado de `hover` mudando o fundo da linha para `surface-container-low`.
*   **Alinhamento:** Números (JetBrains Mono) sempre alinhados à direita para facilitar a comparação visual de valores e estoques.

### Badges de Status (Pills)
*   Formato "pílula" (999px radius).
*   Cores semitransparentes (10% de opacidade da cor de status) com o texto na cor plena. Ex: Status "Disponível" usa fundo Success a 10% e texto Success a 100%.

---

## 6. Do's and Don'ts (Práticas Recomendadas)

### ✅ Pratique
*   **Use Generosidade no Espaço:** Se você acha que o espaçamento está bom, aumente-o em mais 8px. O luxo precisa de ar.
*   **Alinhamento Óptico:** Alinhe ícones dourados no sidebar centralizados visualmente, não apenas matematicamente.
*   **Consistência Numérica:** Use JetBrains Mono para TODO e qualquer número. Isso é a assinatura de precisão do sistema.

### ❌ Evite
*   **Linhas de Grade Visíveis:** Nunca use grades que pareçam uma planilha de Excel. O sistema deve parecer uma aplicação de gerenciamento de ativos de alto valor.
*   **Cores Vibrantes em Excesso:** As únicas cores de destaque permitidas são o Ouro e os tons de Status (Success/Danger). Todo o restante deve orbitar a escala de cinzas quentes e pretos.
*   **Arredondamento Excessivo:** Evite botões totalmente redondos (exceto pills). O raio de 6px-8px mantém a seriedade profissional.

---

**Nota do Diretor:** Este sistema não é apenas uma ferramenta de trabalho, é uma extensão da experiência de luxo que a marca entrega aos seus clientes finais. Cada clique deve ser silencioso, visualmente limpo e intencional.