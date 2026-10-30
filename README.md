# Yasvara manifest

Yasvara on a CodeLinaro QSSI base, with a thin Lineage layer for build infra.

```bash
repo init -u https://github.com/Yasvara/android.git -b yasvara-17.0 --git-lfs
repo sync -c -j$(nproc --all)
```

## Layout

| File | Content |
|---|---|
| `clo.xml` | CLO `LA.QSSI.17.0.r1-12300-qssi.0` manifest, unmodified |
| `snippets/lineage.xml` | Lineage-only projects, Lineage forks of repos CLO leaves alone, AOSP projects CLO lacks. Generated |
| `snippets/yasvara.xml` | Yasvara forks and apps |
| `snippets/trim.xml` | Car, TV, cuttlefish and dev-board targets removed |
| `snippets/pending.xml` | Device, kernel and app repos not published yet; not included |

Include order matters: `trim.xml` comes last so it also drops Lineage forks at trimmed paths.

## CLO tag bump

1. Replace `clo.xml` with the new tag's manifest from `clo/la/la/system/manifest`.
2. Regenerate `snippets/lineage.xml` with `lab/clo-migration/gen-lineage-snippet.py`. It needs fresh `audit.tsv`, `delta.tsv` and `clo-delta-*.tsv` from that directory.
3. Commit as `chore(manifest): bump CLO to <tag>`.

## Porting status

The repos CLO and Lineage both modify (69, list in `lab/clo-migration/port-list.tsv`) track CLO for now. Each one moves to a `Yasvara/<repo>` fork on `yasvara-17.0` once its Lineage and Yasvara commits are ported. Until `frameworks/base`, `build/make` and `build/soong` are ported, the tree syncs but does not build.
