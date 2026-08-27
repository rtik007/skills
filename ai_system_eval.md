# Skill: Causal System Dynamics Evaluator for Agentic & Conversational AI

## Description
A comprehensive evaluation skill that assesses Agentic AI workflows, multi-agent orchestrations, and multi-turn conversational systems by combining **Donella Meadows' System Dynamics (SD)** with the **IBM CLEAR (Causal Learning, Evaluation, and Analysis for Reasoning)** framework.

Rather than treating errors as isolated syntactic failures or superficial log exceptions, this evaluator models execution trajectories as state-determined feedback systems and applies counterfactual interventions ($do$-calculus) to mathematically isolate root causes, quantify error mediation, and evaluate operational resilience.

---

## Theoretical Mapping

| Primitive | System Dynamics Concept | CLEAR Causal Primitive | Agentic / Multi-Agent System | Conversational System |
| :--- | :--- | :--- | :--- | :--- |
| **Stocks (Levels)** | State accumulations over time | Endogenous State Variables ($S_t$) | Working context window, unverified assumptions, error debt. | Dialogue history, accumulated ambiguity debt, user frustration level. |
| **Flows (Rates)** | Decision/activity velocity | State Transition Function ($f_t$) | Tool call frequency, self-correction rate, memory writes. | Turn generation rate, disambiguation rate, deflection/apology rate. |
| **Reinforcing Loops ($+$)** | Self-amplifying growth or collapse | Positive Feedback Graph Cycles | Hallucination spirals, context poisoning, sycophancy echo chambers. | Flawed premise validation, user frustration escalation spirals. |
| **Balancing Loops ($-$)** | Goal-seeking corrective actions | Negative Feedback & Control | Critic-agent retries, guardrails, schema validation loops. | Disambiguation questions, constraint verification, intent narrowing. |
| **Delays** | Latency between action and state change | Multi-Step Causal Lag ($\Delta_{lag}$) | Latent planning bug at Step $k$ surfacing as a tool crash at Step $k+n$. | Misunderstood intent at Turn 1 causing user correction at Turn $n$. |
| **System Boundary** | Internal structure vs. external shocks | Natural Direct vs. Indirect Effects | **Endogenous:** Logic/planning flaws.<br>**Exogenous:** External API 500s.[cite: 2] | **Endogenous:** Bot hallucinations.<br>**Exogenous:** Contradictory user prompts. |

---

## Evaluation Dimensions & Metrics

### 1. Causal & Dynamic Metrics
* **Loop Classification:**
  * `STABLE_CONVERGENT`: Balancing feedback converges smoothly to target state with optimal damping.
  * `REINFORCING_CASCADE`: Positive feedback spiral where unverified assumptions compound into exponential error growth.
  * `BALANCING_OSCILLATION`: Undamped balancing feedback where the agent thrashes between failure states.
  * `OPEN_LOOP_DRIFT`: Execution ignores environment feedback and drifts from the goal state.
* **Causal Delay Lag ($\Delta_{lag}$):** Number of steps between root divergence ($Step_{root}$) and terminal failure ($Step_{terminal}$):
  $$\Delta_{lag} = Step_{terminal} - Step_{root}$$
* **Causal Efficacy Score ($\text{CES}$):** Validates whether $Step_{root}$ is the necessary cause via counterfactual intervention ($do$-calculus)[cite: 2]:
  $$\text{CES}(Step_k) = P(\text{Success} \mid do(State_k = State^*_k)) - P(\text{Success} \mid \text{Observed})$$
* **Causal Mediation Decomposition:** Separates environmental failures from cognitive propagation[cite: 2]:
  * **Natural Direct Effect ($\text{NDE}$):** Tool or infrastructure error independent of reasoning state[cite: 2].
  * **Natural Indirect Effect ($\text{NIE}$):** Failure mediated through upstream context corruption and flawed intermediate decisions[cite: 2].

### 2. Operational & Governance Metrics
* **Task Success (Binary $[0, 1]$):** Trajectory achieved the verified target goal within limits.
* **Task Accuracy Score ($0.0 - 1.0$):** Semantic, factual, and structural compliance with ground truth.
* **System Autonomy Rate ($0.0 - 1.0$):**
  $$\text{Autonomy Rate} = \frac{\text{Autonomous Steps Taken}}{\text{Total Steps (including Human/Fallback Injections)}}$$
* **Graceful Escalation Score ($0.0 - 1.0$):** Quality of state preservation and safety boundary enforcement when handing off to HITL or fallback handlers.
* **Loop Convergence Efficiency ($0.0 - 1.0$):**
  $$\text{Convergence Efficiency} = \frac{\text{Theoretical Minimum Steps}}{\text{Actual Execution Steps}}$$

---

## Input Specification

```json
{
  "system_type": "AGENTIC | CONVERSATIONAL | MULTI_AGENT",
  "task_definition": {
    "goal": "Description of the objective or user intent",
    "ground_truth": "Expected terminal output, state change, or schema assertions",
    "constraints": {
      "max_steps": 10,
      "max_tool_failures": 3,
      "max_token_budget": 8000
    }
  },
  "trajectory": [
    {
      "step_id": 1,
      "actor": "user | planner | executor | critic | assistant",
      "thought_or_intent": "Internal reasoning or inferred intent",
      "action_or_utterance": "Tool invocation payload, code generation, or message",
      "observation_or_feedback": "Tool output, environment return, or user reply",
      "state_snapshot": {
        "scratchpad_summary": "Current working memory state",
        "unverified_assumptions": ["List of unvalidated hypotheses"],
        "error_stock_estimate": 0
      }
    }
  ]
}