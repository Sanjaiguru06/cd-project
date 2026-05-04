# Compiler Phase Visualizer — PRD

## Original Problem Statement
Visualize all phases of a C compiler for complex C programs (nested for-loops, if/else, ||, %, printf with format strings, etc.).
Phase 4 (ICG) must produce TAC with labels & goto matching the user's reference style. Phases 2 & 3 must render proper parse tree and annotated tree.

## User Choices
- Single self-contained HTML file (kept as `/app/frontend/public/compiler_visualizer.html`)
- All 6 phases must work end-to-end
- TAC uses **sequential numbering** (1..N)
- Parse tree and annotated tree are **SVG visual trees** with nodes + edges + type chips
- No preset library (user inputs their own C code)

## Architecture
- Single HTML page with vanilla JS, no build step.
- Pipeline: `tokenize → Parser.parseProgram → semanticAnalysis → generateICG → optimize → generateTarget`.
- AST nodes: `program, preproc, funcdef, block, decl, if, for, while, dowhile, return, exprstmt, binop, assign, unary, postfix, call, index, ternary, id, num, str, char, break, continue, empty`.
- TAC ops: `=, +, -, *, /, %, <, >, <=, >=, ==, !=, &&, ||, !, uminus, goto, if (with .cmp), print, param, call, return, func_begin, func_end, end, break, continue`.
- Layout: Reingold-Tilford-style tree drawing for SVG parse/annotated trees.

## What's Implemented (2026-01)
- ✅ Lexer covering C subset (preprocessor, all operators, strings, char literals, keywords)
- ✅ Recursive-descent parser handling: function defs, declarations (with initializers), nested blocks, if/else, for, while, do-while, return, break, continue, function calls (printf/scanf), array indexing, ternary, full operator precedence
- ✅ Semantic analysis with scoped symbol table, type inference, inline annotation during scoped walk; built-in functions (printf, scanf, etc.) auto-recognized
- ✅ Three-Address Code generation with labels & gotos resolved to sequential line numbers; short-circuit `||`/`&&` jump translation; postfix `i++` produces `t = i+1; i = t`
- ✅ Quadruple, Triple, Indirect-Triple table representations
- ✅ Optimizer: literal-only constant folding + dead temp elimination + goto target remapping
- ✅ Target code generation (MOVI/MOV/ADD/SUB/MUL/DIV/MOD/CMP/JLE/JMP/PUTS/PRINT/CALL/RET/HALT)
- ✅ SVG parse tree with kind-based node coloring & legend
- ✅ SVG annotated tree with type chips (INT/FLOAT/STRING/CHAR)
- ✅ Phase navigation, auto-compile on load, recompile button
- ✅ Tested against 5 sample C programs (nested for, ||, %, *, printf with format)

## Backlog (P2)
- Sample preset gallery (deferred; user opted out)
- More aggressive optimizations (loop-invariant code motion, common subexpression elimination)
- Show "removed/folded" line annotations linking back to original lines
- Collapsible "Compiler Phase Error Reference" table (currently always visible at bottom of Phase 6)

## File Map
- `/app/frontend/public/compiler_visualizer.html` — entire app (~1780 lines)
- Served via React dev server at: `${REACT_APP_BACKEND_URL}/compiler_visualizer.html`
