# 🧭 Plano de Aula – Objeto de Aprendizagem: PONG no Arduino  

---

## 🎯 **Objetivos da Aula**
Ao final da aula, o aluno deverá ser capaz de:

- Entender a lógica de funcionamento do jogo PONG.  
- Compreender como essa lógica é traduzida para um programa embarcado no Arduino.  
- Implementar individualmente cada módulo do jogo:  
  - Controle via potenciômetros  
  - Movimento da bola  
  - Mecânica de colisão  
  - Pontuação e estados do jogo  
- Integrar todas essas partes dentro do `loop()` principal.

---

# 📘 **Parte 1 — Introdução e Explicação Conceitual**  

## **1. Abertura**
- Relembrar brevemente que toda a parte de **Hardware, pinos, TVout e circuito** foi abordada na aula anterior.  
- Destacar que o foco desta aula é **somente a lógica e o código do jogo**.

---

## **2. Estrutura Geral do Projeto**

### **2.1 Programação embarcada**
- O código roda continuamente no `loop()`.  
- Não existe multitarefa tradicional.  
- Não podemos usar `delay()`.  
- O jogo precisa atualizar elementos a cada ciclo.

### **2.2 Diferença entre lógica em PC e embarcada**
- No PC: múltiplos processos, threads, sistema operacional.  
- No Arduino: código direto no microcontrolador, tudo dentro do loop.

---

## **3. Regras do PONG**

- Dois jogadores controlam raquetes verticais.  
- A bola rebate nas bordas superior e inferior.  
- Pontua quando ultrapassa a lateral do campo.  
- O jogo termina quando alguém chega a 5 pontos.

---

## **4. Estrutura da Lógica do Jogo**

### **4.1 Estados do jogo**
- MENU  
- JOGO  
- VITORIA  

---

## **4.2 Controle do Jogador com Potenciômetro**

- Leitura analógica: valores entre 0 e 1023.  
- Conversão para altura da tela com `map()`.  
- Cálculo das posições `X` e `Y`.  
- Desenho da raquete com `TV.draw_line()`.

Pontos a reforçar:
- Limitar a raquete à área da tela.  
- Garantir atualização consistente.

---

## **4.3 Movimento da Bola**
- Variáveis: `bola_x`, `bola_y`, `dir_x`, `dir_y`.  
- Atualizada a cada iteração do loop.  
- Velocidade constante.  
- Movimentação horizontal e vertical.

---

## **4.4 Mecânica de Colisões**

### Com a tela:
- Topo → inverter `Y`.  
- Base → inverter `Y`.

### Com as raquetes:
- Topo → `Y = 1`  
- Meio → `Y = 0`  
- Base → `Y = -1`  

---

## **4.5 Pontuação e Reinício**
- Detectar quando a bola sai pelas laterais.  
- Incrementar pontuação correspondente.  
- Reiniciar bola no centro.  
- Detectar quando um jogador atinge 5 pontos.  
- Alterar estado para VITÓRIA.

---

# 👨‍🏫 **Parte 2 — Revisão do Código e Comentários**  

## **5. Apresentação do Código Pronto**

---

## **5.1 Erro comum: mapeamento do potenciômetro**
Problemas típicos:
- Mapeamento invertido.  
- Raquete fora da tela.  
- Valores extremos não tratados.

---

## **5.2 Erro comum: bola travando ou se movendo errado**
- Alunos esquecem de atualizar tanto X quanto Y.  
- `dir_x` e `dir_y` mal configurados.  
- Velocidade zerada sem querer.

---

## **5.3 Erro comum: colisão incorreta**
- Comparações mal ordenadas.  
- Razão de colisão não respeita as regiões da raquete.  
- Bola atravessando a raquete.

---

## **5.4 Erro comum: pontuação não funcionando**
- Verificar colisão antes de verificar gol.  
- Reinício da bola incorreto.  
- Estado VITÓRIA nunca ativado.

---

## **5.5 Erro comum: organização ruim do loop**
Refaça o conselho:

> “Misturar a lógica do jogo com a do menu deixa o código confuso.  
> A estrutura recomendada é:”

```cpp
switch (estado) {
  case MENU:
    desenha_menu();
    break;
  case JOGO:
    atualiza_jogo();
    break;
  case VITORIA:
    mostra_vencedor();
    break;
}
