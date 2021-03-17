# Evaluation of proposals for Authorization

| WAC | WAC+ | ACP | Requirement | Warrant | 
| --- | --- | --- | --- | --- |
| ✅ | ✅ | ✅ | [3.1.1. The system shall allow access to be limited based on the identity of the agent.](https://solid.github.io/authorization-panel/authorization-ucr/#req-agent-identity) | WAC: 1. in [issue 176](https://github.com/solid/authorization-panel/issues/176). |
| ❌ | ✅ | ❌  | [3.1.2. The system shall allow access to be limited based on the identity of the agent, only when that identity is issued by a trusted identity provider.](https://solid.github.io/authorization-panel/authorization-ucr/#req-trusted-identity) | WAC+: 2. in [issue 176](https://github.com/solid/authorization-panel/issues/176). | |
| 🔸 | 🔸 | 🔸 | [3.1.3. The system shall allow access to be limited to an agent based on the agent’s membership in a certain group of agents.](https://solid.github.io/authorization-panel/authorization-ucr/#req-agent-group) | |
| 🔸 | 🔸 | 🔸 | [3.1.4. The system shall allow access to be limited to an agent based on the client application in use by the agent.](https://solid.github.io/authorization-panel/authorization-ucr/#req-application) | |
| 🔸 | 🔸 | 🔸 | [3.1.5. The system shall allow an agent to limit modes and/or conditions of access for a given client application in their use for a resource or collection that they have been granted access to.](https://solid.github.io/authorization-panel/authorization-ucr/#req-client-constrained) | |
| 🔸 | 🔸 | 🔸 | [3.1.6. The system shall allow access to be permitted for any unauthenticated or authenticated agent.](https://solid.github.io/authorization-panel/authorization-ucr/#req-public) | |
| 🔸 | 🔸 | 🔸 | [3.1.6. The system shall allow access to be permitted for any unauthenticated or authenticated agent.](https://solid.github.io/authorization-panel/authorization-ucr/#req-public) | |
| 🔸 | 🔸 | 🔸 | [3.1.7. The system shall allow access to be limited to any authenticated agent.](https://solid.github.io/authorization-panel/authorization-ucr/#req-authenticated) | |
| 🔸 | ✅ | 🔸 | [3.2.2. The system shall ensure that there are practical and efficient mechanism available for the client to determine an appropriate credential to present for access to a given resource.](https://solid.github.io/authorization-panel/authorization-ucr/#req-vc-determine) | [issue 189: ACLs on ACLs for WAC](https://github.com/solid/authorization-panel/issues/189) |
| 🔸 | 🔸 | 🔸 | []() | |

✅ Requirement is fulfilled
❌ Requirement is no met
🔸 Status of requirement fulfillment to be determined
