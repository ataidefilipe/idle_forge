# **IDLE FORGE REALM – GAME DESIGN & PLANO DE DESENVOLVIMENTO COMPLETO**  
**v1.0 – MVP a Full Launch**  
**Data**: 07 de Novembro de 2025  
**Equipe Mínima Viável**: 1 Dev (Core), 1 Artist (2D), 1 Designer (Balance), 1 QA/Community  

---

## **1. VISÃO GERAL DO JOGO (PITCH OFICIAL)**

> **Idle Forge Realm** é um **idle RPG multiplayer com economia 100% player-driven**.  
> Sem XP. Sem níveis. **Só equipamento.**  
> 5 profissões de extração → 25 matérias-primas.  
> 7 mestres do craft → 35 itens modulares.  
> Mercado, degradação e combate automático em trios.  
> **Jogue 5 minutos ou 5 horas. Offline ou online — o mundo segue girando.**

---

## **2. PILARES DO DESIGN**

| Pilar | Descrição |
|------|----------|
| **Economia Viva** | Tudo é feito por jogadores. NPCs só compram/vendem basics com preço dinâmico. |
| **Progressão por Gear** | Poder = qualidade do equipamento. Craft modular + runas PvE. |
| **Idle com Profundidade** | Offline seguro (3 chars), online burst (energia). |
| **Simplicidade + Escala** | 5×5 extração + 7×5 craft = sistema fechado, mas com milhares de combinações. |
| **Combate Tático Automático** | Formação de 3, posicionamento, gatilhos. |

---

## **3. ESCOPO DO MVP (Minimum Viable Product)**

| Feature | Inclusa no MVP? | Prioridade |
|--------|------------------|----------|
| 3 Extração + 3 Craft | Yes | Alta |
| 15 Materiais + 15 Itens | Yes | Alta |
| Mercado Básico (Auction) | Yes | Alta |
| 3 Chars + Energia + Offline | Yes | Alta |
| Combate PvE Automático | Yes | Alta |
| Degradação + Reparo | Yes | Alta |
| Biomas N1-N3 | Yes | Média |
| Runas Básicas (PvE Drop) | Yes | Média |
| PvP Arenas | No | Pós-MVP |
| Guildas/Territórios | No | Fase 2 |

> **MVP = 3 meses (1 dev + 1 artist)**  
> **Objetivo**: Provar loop econômico + retenção idle.

---

## **4. ARQUITETURA TÉCNICA**

| Camada | Tecnologia |
|-------|-----------|
| **Cliente** | Unity 2023 LTS (WebGL + Mobile iOS/Android) |
| **Servidor** | Node.js + Socket.IO (real-time mercado/combate) |
| **Banco** | PostgreSQL (player data, market, items) |
| **Auth** | Firebase Auth |
| **Backend** | Server Authoritative (anti-cheat) |
| **Deploy** | AWS EC2 + S3 (assets) |

---

## **5. PROGRESSÃO DO JOGO (Fases de Desenvolvimento)**

### **FASE 0 – PROTÓTIPO (2 semanas)**
- [ ] 1 Extração (Minerador) + 1 Craft (Ferreiro)  
- [ ] 1 Material → 1 Espada modular  
- [ ] Offline farming (1 char)  
- [ ] Mercado local (NPC fixo)  
- [ ] Combate PvE (1 monstro)  
> **Teste**: Loop funciona? Craft → Equip → Farm → Degrade → Repare.

---

### **FASE 1 – MVP (3 meses)**
```text
Mês 1: Core Systems
└─ Energia, 3 chars, offline, UI base, 3 extrações, 3 crafts
Mês 2: Economia + Combate
└─ Mercado auction, PvE biomas N1-N3, runas, combate trio
Mês 3: Polish + Teste
└─ Balance, UI/UX, onboarding, analytics
```
> **Lançamento Soft (WebGL + Android)**

---

### **FASE 2 – EXPANSÃO (6 meses)**
- [ ] +2 Extração (Herbalista, Caçador)  
- [ ] +4 Craft (Armorista, Costureiro, Arqueiro, Cajadeiro)  
- [ ] Biomas N4-N7  
- [ ] PvP Arenas (sem loot)  
- [ ] Leaderboards (riqueza, craft raro)  
- [ ] Cosméticos (F2P)  
> **Lançamento Full (iOS + Steam)**

---

### **FASE 3 – ENDGAME (12+ meses)**
- [ ] Biomas N8-N10  
- [ ] Raids cooperativos (PvE boss)  
- [ ] Guildas com taxação de bioma  
- [ ] Eventos sazonais (invasão → escassez)  
- [ ] API pública (mods, bots de mercado)

---

## **6. BALANCE INICIAL (MVP)**

| Mecânica | Valor Base |
|--------|-----------|
| Energia Total | 1000/dia (1 por minuto) |
| Offline Yield | 100% → 50% em 24h |
| Degradação | -1% por uso (durabilidade base 100) |
| Reparo | 20% materiais |
| Craft RNG | ±10% dos atributos base |
| Mercado Taxa | 2% |
| Poção Energia | +500 (cooldown 1h) |

---

## **7. UI/UX – FLUXO DO JOGADOR**

```
[Login] → [Seleciona 3 chars ativos] → 
→ [Farm Ativo ou Define Offline] → 
→ [Craft / Mercado / Combate] → 
→ [Degradação → Reparo ou Venda] → 
→ [Melhora gear → Mais farm] → Ciclo
```

**Telas Principais**:
1. **Dashboard**: 3 chars + energia + offline status  
2. **Farm**: Bioma + ferramenta + yield  
3. **Craft**: Receita + rolagem + preview stats  
4. **Mercado**: Filtros + leilão  
5. **Combate**: Formação + auto-resolve + relatório  
6. **Personagem**: Slots + modular + runas  

---

## **8. MONETIZAÇÃO (F2P ÉTICO)**

| Modelo | Descrição |
|-------|----------|
| **Cosméticos** | Skins de gear, animações, ícones de char |
| **Boost Temporário** | +50% energia por 1h (não stack) |
| **Pacote Inicial** | 1 char extra + skin (one-time) |
| **Sem Pay-to-Win** | Nenhum item afeta balance |

> **Meta ARPU**: $2-3 / jogador pagante

---

## **9. MÉTRICAS DE SUCESSO (KPIs)**

| Métrica | Meta MVP (3 meses) |
|--------|-------------------|
| DAU | 1.000 |
| Retenção D7 | 40% |
| % Jogadores com Craft | 70% |
| Volume Mercado (diário) | 10.000 transações |
| Tempo Médio Sessão | 8 min (idle) / 25 min (ativo) |

---

## **10. ROADMAP RESUMIDO**

| Mês | Milestone |
|-----|----------|
| 0-0.5 | Protótipo interno |
| 1-3 | MVP (Soft Launch Android) |
| 4-6 | Polish + iOS + WebGL |
| 7-9 | Expansão (5×5 completo) |
| 10-12 | PvP + Leaderboards |
| 13+ | Guildas + Endgame |

---

## **11. RISCOS & MITIGAÇÃO**

| Risco | Mitigação |
|------|----------|
| Economia desbalanceada | NPCs com preço dinâmico + taxas |
| Bottleneck de craft | Mercado define → migração natural |
| Abuso de offline | Cansaço + limite de 3 chars |
| Churn inicial | Onboarding com craft grátis N1 |

---

## **12. PRÓXIMOS PASSOS IMEDIATOS**

1. [ ] **Wireframe UI** (Figma) – 1 semana  
2. [ ] **Protótipo Unity** (1 extração + 1 craft) – 2 semanas  
3. [ ] **Planilha de Balance** (Google Sheets)  
4. [ ] **Servidor Node.js** (auth + mercado mock)  
5. [ ] **Teste com 10 amigos** → feedback loop

---

**IDLE FORGE REALM**  
**Não é um jogo. É uma economia que você joga.**  
**Pronto para forjar.**

> **"O martelo está na sua mão. O resto? É com o mercado."**  
> **— Dev Note**

---  
**Fim do Planejamento. Próximo: Execução.**