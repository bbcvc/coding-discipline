# coding-discipline

```xml
<coding-discipline name="Public Paradigm First" scope="js/ts: frontend | backend | agent">

  <spirit>
    Code should be a public language, not a private monologue.
    When AI handcrafts everything from scratch, it effectively reinvents a new
    "private language" every time—creating style drift and making maintenance difficult for humans.
    This discipline is not a dogma, but a set of ideals worth striving toward:
    Through composition, unified contracts, and single sources of truth,
    converge every reusable capability into a shared paradigm,
    using the language of the community to resist the entropy of generation.
    — You are both a consumer of wheels and a citizen of the ecosystem.
  </spirit>

  <supreme-principle name="Ultimate Arbitration: Resolve all conflicts according to these principles">
    Working is better than perfect.
    Focus is better than features.
    Compatibility is better than purity.
    Simplicity is better than anything.
    <note>
      Whenever any downstream rule conflicts with simplicity, simplicity wins.
      This overrides all mechanical red lines (e.g. "never use a library if the code is under N lines"):
      judge by taste, not by numbers.
    </note>
  </supreme-principle>

  <core name="Composition Above All">
    <composition>
      The primary goal is not how many libraries are used,
      but whether components can be assembled like pipelines.
      Prefer composition; reject inheritance-based extension.
    </composition>

    <universal-interface>
      Prefer capabilities that define a unified contract or composable interface,
      because they provide the strongest convergence force.
      In the Node ecosystem, the universal interface has evolved from Streams
      to the contract layer itself.
      Zod connects frontend, backend, and agents not merely because it is mature,
      but because it provides a cross-platform contract.
    </universal-interface>

    <producer-consumer-unity>
      Consuming dependencies and publishing modules are two expressions of the same philosophy;
      neither is superior to the other.
      A small, focused module written quickly for yourself today
      may become someone else's dependency tomorrow.
      → Therefore, writing reusable internal modules is not a fallback plan,
      but a normal manifestation of this philosophy.
    </producer-consumer-unity>
  </core>

  <mindset name="Decomposition: Business Is the Problem, Generality Is the Means">
    Requirements originate entirely from business needs,
    but should never be implemented as a single indivisible block.
    First decompose the business requirement into implementation capabilities,
    then classify each capability individually.

    <example>
      Requirement: "Smoothly display 100,000 orders with filtering and real-time updates"
      ├─ Virtualized list rendering → General capability → @tanstack/virtual
      ├─ Requesting / caching / refreshing → General capability → @tanstack/query
      ├─ Filter validation → General capability → zod
      └─ Order domain rules → Actual business logic → Custom implementation
    </example>
  </mindset>

  <decision-rule name="Where Each Capability Belongs">
    <option name="Use a Mature Library">
      General capability + mature library exists
      → adopt it and follow its idiomatic usage.
      This is the default choice.
    </option>

    <option name="Quick Internal Module">
      General capability + no suitable library exists
      → quickly encapsulate it as a small module with a single responsibility
      and a clear interface.

      "Write modules quickly, to meet your needs, with just a few tests."

      If the same capability appears again, it must be reused.
      Reimplementation is forbidden.
      It may later be published as an internal package.
    </option>

    <option name="Business-Specific Code">
      True domain logic
      → implement it yourself,
      following existing project conventions
      and keeping it decoupled from the general-purpose layer.
    </option>

    <burden-of-proof>
      The burden of proof is reversed:
      using an existing paradigm (a library or an internal module)
      is the default and requires no justification;
      reinventing a general-purpose capability requires justification.
    </burden-of-proof>
  </decision-rule>

  <selection-criteria name="What Makes a Library Worth Using">
    <criterion weight="highest">
      Provides a unified contract or composable interface
      that can integrate with other components.
    </criterion>

    <criterion>
      Generality: solves problems everyone has,
      not just your business.
    </criterion>

    <criterion>
      Error-proneness: difficult areas where hand-rolled implementations
      are likely to introduce bugs
      (caching, concurrency, time zones, security, virtualization, etc.).
    </criterion>

    <criterion>
      Maturity: actively maintained, widely adopted,
      and well documented
      (reducing supply-chain and maintenance risks).
    </criterion>
  </selection-criteria>

  <single-source name="Single Source of Truth for General Capabilities (Deep DRY)">
    Every piece of knowledge or capability in a system
    should have exactly one authoritative representation.

    If it can converge into a library, use a library.
    Otherwise, converge it into an internal module.

    Never repeatedly reinvent the same capability
    through ad-hoc inline implementations.
  </single-source>

  <resolve-docs name="Before Using a Library, Reconnect package.json to Idiomatic Usage">
    package.json only declares what is installed and which version;
    it does not explain how the library should be used.

    If a model fills in the gaps from memory,
    outdated knowledge, hallucinations, and version mismatches are inevitable.

    This leads to another form of chaos:
    superficially using a library while actually misusing it.

    Therefore, before adopting any library,
    obtain the authoritative usage documentation
    corresponding to the version installed in the project.

    <source priority="1">
      Documentation MCPs such as Context7:
      real-time retrieval aligned with the installed version.
      Preferred source.
    </source>

    <source priority="2">
      The library's llms.txt or llms-full.txt (if available):
      AI-friendly official documentation indexes.
    </source>

    <source priority="3">
      Local node_modules type definitions (.d.ts) and README files:
      always available and perfectly version-aligned.
      Type signatures are the most reliable source of truth for intended usage.
    </source>

    <rule>
      package.json is the sole source of truth for version contracts;
      all documentation must be aligned with it.
    </rule>

    <rule>
      The authoritative source of usage knowledge is
      version-locked documentation, never model memory.
    </rule>
  </resolve-docs>

  <usage-constraint name="Using a Library Does Not Mean Abusing a Library">
    After obtaining authoritative usage information through resolve-docs,
    structure code according to the library's idioms.
    Otherwise the discipline is not being followed.

    <anti-pattern>
      Importing @tanstack/query but still manually fetching in useEffect.
    </anti-pattern>

    <anti-pattern>
      Importing react-hook-form but still managing form state manually.
    </anti-pattern>
  </usage-constraint>

  <arsenal name="Domain-Specific Toolkit (Non-Exhaustive, Subject to Simplicity and Unified Contracts)">
    <frontend>
      Request caching=@tanstack/query |
      Virtualized lists=@tanstack/virtual |
      Forms=react-hook-form+zod |
      State=zustand/jotai |
      Components=shadcn/antd |
      Drag-and-drop=dnd-kit |
      Animation=framer-motion
    </frontend>

    <backend>
      Framework=Hono/Fastify |
      Validation=zod |
      ORM=Drizzle/Prisma |
      Authentication=better-auth/Lucia |
      Queue=BullMQ |
      Date handling=date-fns/Temporal
    </backend>

    <agent>
      LLM=@anthropic-ai/sdk / ai(vercel) |
      Orchestration=Agent SDK/LangGraph |
      Structured output=zod+tool use |
      Tool input contracts=zod
    </agent>

    <meta-rule>
      Prefer libraries that define contracts and can be reused across layers.
      Zod's ability to span frontend, backend, and agents is the archetypal example.
    </meta-rule>
  </arsenal>

  <anti-patterns name="Anti-Patterns (Not Absolute Rules; Simplicity Has Final Authority)">
    <item>
      Implementing a large file that mixes general-purpose and business logic
      without decomposition.
    </item>

    <item>
      Reinventing general-purpose capabilities
      without first investigating mature libraries.
    </item>

    <item>
      Introducing a library without performing resolve-docs,
      then relying on memory and writing outdated or hallucinated APIs.
    </item>

    <item>
      Introducing a library but ignoring its idiomatic usage patterns.
    </item>

    <item>
      Treating reusable internal modules as a reluctant fallback
      rather than a normal contribution to the ecosystem.
    </item>
  </anti-patterns>

</coding-discipline>
```
