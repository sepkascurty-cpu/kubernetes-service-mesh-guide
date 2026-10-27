# Quiz: Kubernetes Service Mesh

Test yourself before peeking — answers are collapsed below each question.

## 1. What is the difference between the data plane and the control plane in a service mesh?

<details>
<summary>Answer</summary>

The **data plane** is the set of sidecar proxies (e.g. Envoy) that sit next to every pod and actually handle traffic — routing, retries, encryption, telemetry. The **control plane** (e.g. Istiod) configures those proxies: it discovers services, issues certificates, and pushes routing and policy rules. Your application code talks to neither directly.
</details>

## 2. A VirtualService sends 90% of traffic to v1 and 10% to v2. What Istio object defines what "v1" and "v2" actually are?

<details>
<summary>Answer</summary>

A **DestinationRule**. It defines subsets (v1, v2) by pod labels and attaches traffic policies such as load balancing, connection-pool limits, and outlier detection (circuit breaking). The VirtualService decides *where* traffic goes; the DestinationRule defines *what it goes to* and *how it behaves*.
</details>

## 3. You enabled mTLS in STRICT mode. Does that mean only authorized services can call your API? Why or why not?

<details>
<summary>Answer</summary>

No. mTLS provides **authentication and encryption**: it proves *who* is calling and encrypts the traffic. It does not decide *whether* that caller is allowed to use a particular endpoint. That is **authorization**, handled by an `AuthorizationPolicy` (who, from which namespace/service account, may call which methods and paths).
</details>

## 4. Why can unlimited retries make an outage worse, and what limits them?

<details>
<summary>Answer</summary>

Each retry multiplies load on an already struggling service — a retry storm can turn a partial failure into a total one. Limit retries with an **attempt count**, a **per-try timeout**, and a **retry budget** (only retry a fraction of requests), and combine them with circuit breaking so a failing subset gets ejected instead of retried forever.
</details>

## 5. When should you NOT adopt a service mesh?

<details>
<summary>Answer</summary>

When you don't have the problem it solves: a handful of services, a single team, or no concrete need for mTLS, canary releases, or uniform observability. A mesh adds operational complexity and per-hop latency (~1–3 ms) plus sidecar resource overhead — adopt it for a specific pain (security, progressive delivery, multi-team observability), not as a default.
</details>
