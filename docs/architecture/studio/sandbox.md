---
summary: The sandbox runtime for studio-generated code - isolates, capability tokens, egress.
read_when: Changing how generated code runs or what it can reach.
related: [app-studio.md, ../apps/sdk.md]
---
# Sandbox runtime

- Code steps run in V8 isolates (isolated-vm or workerd) behind the `AppRuntime` port: no filesystem, no raw sockets, caps on CPU, memory and wall-clock per call. Created on demand, not containers.
- The only way out is the SDK over RPC with a short-lived **capability token** scoped to instance, app version and granted permissions. Same `ctx` shape as in-process apps.
- Outbound HTTP only through the egress proxy, only to manifest-declared hosts, logged.
- Generated UI components run in a sandboxed iframe on a separate origin with a narrow postMessage bridge.
- Upgrade path if ever needed: gVisor or Firecracker micro-VMs for native packages or long-lived processes.
