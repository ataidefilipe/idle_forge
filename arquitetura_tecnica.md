# **IDLE FORGE REALM – ARQUITETURA TÉCNICA & RISCOS PLAYER-FIRST**  
**Por: Dev Sênior (15 anos em idle/economia multiplayer – ex-RuneScape, EVE Online, Albion Online)**  
**Foco: UX First, Escalabilidade, Anti-Frustração, Segurança**

---

## **1. PILARES TÉCNICOS (PLAYER-FIRST)**

| Pilar | Por que importa pro jogador |
|------|-----------------------------|
| **Latência < 200ms** | Mercado e combate **não podem travar** quando 1000 players craftam. |
| **Offline 100% confiável** | Jogador **nunca perde progresso** por sair. |
| **Anti-Cheat Server-Authoritative** | **Ninguém flooda mercado** com bots. |
| **Mobile-First UI** | 80% dos idle players jogam **no celular**. |
| **Onboarding < 3 min** | Se não entender em 3 min, **desinstala**. |

---

# **2. ARQUITETURA TÉCNICA DETALHADA**

```
[Cliente (Unity WebGL/Mobile)]
      ↓ (WebSocket / HTTPS)
[Gateway (Node.js + Socket.IO Cluster)]
      ↓
[Microservices (Docker + Kubernetes)]
   ├─ Auth Service (Firebase)
   ├─ Player Service (PostgreSQL + Redis Cache)
   ├─ Market Service (Order Book In-Memory + DB)
   ├─ Combat Service (Simulation Queue)
   ├─ Offline Service (Cron Jobs)
   └─ Analytics (ClickHouse)
      ↓
[DB Master (PostgreSQL) + Read Replicas]
```

---

## **3. FLUXO DE DADOS CRÍTICO (EXEMPLO: CRAFT → MERCADO)**

```mermaid
graph TD
    A[Player clica "Craft Espada"] --> B{Valida no Client?}
    B -->|Sim| C[Envia ao Server (WebSocket)]
    C --> D[Server: Valida energia, materiais, skill]
    D -->|OK| E[Simula Craft (RNG server-side)]
    E --> F[Salva Item no DB + Deduz materiais]
    F --> G[Player recebe item + notificação]
    G --> H[Player lista no mercado]
    H --> I[Order Book atualiza em tempo real (todos clients)]
```

> **Risco evitado**: Client nunca gera item. **Zero chance de duping.**

---

## **4. ESTRUTURA DE DADOS (DB SCHEMA SIMPLIFICADO)**

```sql
TABLE players (id, energy, last_online)
TABLE characters (id, player_id, profession, skill_level, active)
TABLE items (id, owner_id, template_id, parts[], durability, stats)
TABLE market_orders (id, item_id, price, type, expires_at)
TABLE offline_farms (char_id, biome_id, start_time, efficiency)
```

- **Items como JSONB** → modularidade infinita sem schema hell.  
- **Market Orders com TTL** → auto-expira leilões.  
- **Redis para hot data** (market, energy regen, combat queue).

---

## **5. RISCOS CRÍTICOS (PLAYER-FIRST) & MITIGAÇÃO**

| Risco | Impacto no Jogador | Solução Técnica |
|------|---------------------|-----------------|
| **1. Offline perde progresso** | "Saí do jogo, perdi 12h de farm" | **Cron jobs a cada 5 min** + **snapshot de estado** no DB. |
| **2. Mercado crasha com 10k players** | "Não consigo vender nada" | **Order Book in-memory (Redis)** + **sharding por item type**. |
| **3. Craft RNG injusto** | "Gastei 10h e veio lixo" | **RNG server-side + pity system** (ex: +5% chance a cada falha). |
| **4. Bot flooda mercado** | "Preços quebram, economia morre" | **Rate limit por IP/char** + **CAPTCHA em craft em massa** + **análise de comportamento**. |
| **5. Mobile trava em combate** | "Combate de 30s vira 2 min" | **Combate simulado no server** → cliente só anima resultado. |
| **6. Player não entende craft** | "O que combina com o quê?" | **UI com preview 3D + filtro "melhor receita"** + **tooltips com % sucesso**. |
| **7. Energia acaba rápido** | "Só jogo 10 min por dia" | **Regen passiva + poções craftáveis** + **offline yield compensa**. |

---

## **6. UX FIRST – REGRAS DE OURO**

| Regra | Implementação |
|------|---------------|
| **Nunca espere** | Todas ações < 300ms. Loading → skeleton UI. |
| **Feedback imediato** | Craft → animação + som + pop-up com stats. |
| **Transparência total** | Preço médio do item no mercado **sempre visível**. |
| **Zero microgestão** | Offline auto-config: "Farmar bioma X com ferramenta Y". |
| **Mobile = prioridade** | Botões > 48dp, scroll com uma mão, dark mode nativo. |
| **Onboarding gamificado** | Tutorial = **missão real**: "Minere 10 ferro → craft espada → equipe". |

---

## **7. ANTI-FRUSTRAÇÃO (PSICOLOGIA DO JOGADOR)**

| Problema Comum | Solução |
|---------------|--------|
| "Não sei o que fazer" | **Dashboard com 3 ações sugeridas**: Farm, Craft, Mercado. |
| "Perdi tudo no reparo" | **Recycle garante 50% materiais** + preview de custo. |
| "Mercado vazio" | **NPC seed orders** nos primeiros 7 dias do servidor. |
| "Combate demora" | **Auto-skip animação** + relatório em 3 linhas. |

---

## **8. ESCALABILIDADE & OPERAÇÃO**

| Escenario | Infra Necessária |
|----------|-----------------|
| **10k DAU** | 2x EC2 t3.medium + 1x RDS |
| **100k DAU** | Kubernetes (EKS) + 8x workers + Read Replicas |
| **1M DAU** | Sharding por região + CDN (CloudFront) |

> **Custo inicial (MVP)**: ~$300/mês (AWS)  
> **Custo 100k DAU**: ~$3k/mês

---

## **9. SEGURANÇA & ANTI-CHEAT**

| Camada | Medida |
|-------|-------|
| **Client** | Obfuscation (Unity IL2CPP) + assinatura de pacotes |
| **Server** | **Todas simulações server-side** (craft, farm, combate) |
| **Exploit** | Rate limit: 1 craft/2s, 1 mercado/1s |
| **Bots** | Behavioral ML (ClickHouse + Python) → ban automático |
| **Duping** | Item ID único + audit log |

---

## **10. PIPELINE DE DEPLOY (CI/CD)**

```yaml
GitHub → GitHub Actions → 
  Unit Tests → Build (WebGL/Android/iOS) → 
  Staging (internal) → 
  Canary (5% players) → 
  Production
```

> **Hotfix em < 15 min**  
> **Rollback automático** se crash rate > 1%

---

## **11. PRÓXIMOS PASSOS TÉCNICOS (PRIMEIRA SEMANA)**

| Tarefa | Responsável | Prazo |
|-------|-------------|------|
| Protótipo Unity (1 craft + mercado local) | Dev | 5 dias |
| Server Node.js + PostgreSQL (auth + itens) | Dev | 7 dias |
| UI Mobile (Figma → Unity) | Artist | 7 dias |
| Balance Sheet (Google Sheets) | Designer | 3 dias |
| Teste com 5 players reais | QA | 7 dias |

---

## **12. RESUMO: VISÃO PLAYER-FIRST**

> **O jogador não quer tecnologia. Quer sentir que está construindo algo real.**  
> - **Se o mercado responde em 1s → ele sente o mundo vivo.**  
> - **Se o offline entrega exatamente o prometido → ele confia.**  
> - **Se o craft falha, mas ele entende por quê → ele tenta de novo.**  

**IDLE FORGE REALM não é sobre código. É sobre confiança, transparência e um mundo que funciona mesmo quando ele está dormindo.**

---

**Status: PRONTO PARA CODAR.**  
**Primeiro commit: `feat: minerador + ferreiro + mercado local`**

> **"Construa o sistema que você gostaria de jogar por 5 anos."**  
> — Dev Sênior, 2025

---  