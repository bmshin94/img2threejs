# img2threejs (img2threejs/img2threejs)

## 프로젝트 개요
평범한 사진 한 장을 넣으면 복잡한 3D 그래픽 파일 없이 오직 순수한 코드로 3D 입체 물체를 재현해내는 "사진 기반 3D 코드 변환기"
무거운 3D 모델링 프로그램을 다루지 못해도 웹 브라우저 안에서 자유롭게 회전하고 반응하는 3D 오브젝트를 자동 생성
인터랙티브 웹 디자인, 온라인 쇼룸, 디지털 쇼케이스에 시각적 혁신을 불어넣어 주는 놀라운 그래픽 마법사

## 핵심 특징 & 추천 분야
- 사진기반3D변환
- 코드기반입체생성
- 웹3D오브젝트
- 모델링없는3D
- 인터랙티브쇼룸

---
*이 문서는 오픈소스 큐레이터(Curator-Agent)에 의해 자동 생성된 가이드 문서입니다.*


---
## 기존 CLAUDE.md 내용

# Shared project instructions

This repository is the canonical source for the `img2threejs` skill. Host entrypoints
(`~/.claude/skills/img2threejs`, `~/.codex/skills/img2threejs`) should be symlinks to one checkout —
never independent copies, or the two hosts drift apart silently.

## Change rules

- Preserve the code-only procedural Three.js contract; do not silently download meshes or art packs.
- Keep claims honest: distinguish implemented capability from roadmap or design-only documentation.
- Treat `forge/` as deterministic tooling and `grimoire/` as routed reference material.
- Keep backward compatibility for existing sculpt specs unless a migration is explicitly planned.
- When changing schema, gates, generators, or review behavior, add or update focused tests.
- Keep `SKILL.md`, `README.md`, `CHANGELOG.md`, and `ROADMAP.md` consistent when release-facing
  behavior changes.
- Reference the companion showcase through `IMG2THREEJS_SHOWCASE_ROOT`, never an absolute path — a
  path that only exists on one machine passes there and fails everywhere else, CI included.

## Verification

```bash
python3 -m unittest discover -s forge/tests -p 'test_*.py'
```

Set `IMG2THREEJS_SHOWCASE_ROOT` to a showcase checkout to include the TypeScript typecheck gates;
without it they skip, and a green run has not proven the emitted Three.js compiles. Add
`IMG2THREEJS_REQUIRE_SHOWCASE=1` to turn that skip into a failure.

Do not report completion without reading the fresh outputs. For visual reconstruction changes,
structural tests and screenshot/reference-loop validation are separate required gates.

## Mandatory visual screenshot gate

For every visual reconstruction task, a readable screenshot is a hard prerequisite for visual
implementation claims and completion:

1. Before accepting visual results, verify that the browser/screenshot tooling is installed,
   authenticated, reachable, and able to capture the running showcase.
2. Save fresh PNG/JPEG screenshots inside the workspace, including the fixed reference view and the
   required orbit views. Inline previews alone are not evidence.
3. Read the saved screenshots back with an image-capable tool and verify they contain the rendered
   model at the expected dimensions. A screenshot that cannot be opened or visually read is a failed
   gate.
4. Produce and retain a side-by-side reference/render comparison, semantic image scoring,
   pixel/feature comparison, and the `diagnose_render.py` output for the saved render before
   reporting visual validation.
5. If capture, file write, readback, comparison, scoring, or diagnosis fails, stop the visual
   workflow and repair the tooling first. Do not infer visual evidence from runtime readiness,
   structural tests, inline previews, or code review, and do not claim the visual gate passed.

**Scope of this gate.** It governs the Three.js reconstruction this repository builds — the
procedural model and its self-correction loop. An exported artifact produced by an explicitly-selected
emission target (`--target <kind>`) is a different thing: it is verified to the base's own stated
limit — container/structure check where a prober exists, existence/size/location/determinism
otherwise — and is never reported under this gate's completion vocabulary. The visual fidelity of an
export, if any is claimed, is that target plugin's own gate's responsibility, not this one's.
