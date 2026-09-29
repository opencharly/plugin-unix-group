# plugin-unix-group

Unix group provisioning for OpenCharly — the `unix_group:` verb.

The verb is a multi-role state-provision verb: it **checks** a group with
`getent group` through the live check engine and compares the GID, and it **acts**
by rendering an idempotent `groupadd`. There are no matchers — it does a direct
field comparison.

It is a host-coupled verb on the SDK kit contract (`CheckVerbProvider` +
`ProvisionActor`), so it is **compiled-in only**.

## What it provides

| Capability | Surface |
|---|---|
| `verb:unix_group` | the `unix_group:` typed step — probe a group and render its creation |

## How to use it

Compose the plugin candy in a box or check bed's `candy:` list:

```yaml
- '@github.com/opencharly/plugin-unix-group/candy/plugin-unix-group:<tag>'
```

Then author the verb in a plan:

```yaml
- check: the root group exists with gid 0
  id: unix-group-root
  unix_group: {unix_group: root, gid: 0}
  context: [runtime]
```

## Layout

- `candy/plugin-unix-group/` — the plugin module: `plugin.go` (the verb +
  `NewCheckVerb()`/`NewMeta()`), `schema/unix_group.cue` (the self-contained
  `#UnixGroupInput`), `params/cue_types_gen.go`, `cmd/serve/main.go`.
- `charly.yml` — the root project manifest (`discover: candy`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.

## Related

- Owning skill: `/charly-internals:plugin` — the plugin/provider model and the
  multi-role state-provision contract this verb follows. This candy carries no
  `skill:` entity of its own; the gap is tracked in
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
- `/charly-core:service` — the sibling init-agnostic service-provision verb.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI.
