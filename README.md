# Ethernaut Audit Reports

Welcome to my **Ethernaut Solidity audit repository**. This repo contains **step-by-step audits** of Ethernaut levels, highlighting vulnerabilities, reasoning, proofs-of-concept (PoCs), impact assessment, and mitigation suggestions.  

The goal is to **practice manual auditing**, understand contract logic deeply, and document findings clearly, just like professional bounty reports.

---

## Methodology

For each level, I follow a structured approach:

1. **Read & Understand the Contract:** Identify key variables, functions, and ownership logic.  
2. **Reason About Vulnerabilities:** Think through how contracts can be exploited without immediately using automated tools.  
3. **Test Locally:** Use Remix, Foundry, or a local testnet to confirm potential exploits.  
4. **Document Findings:** Write a structured audit report including:
   - Summary  
   - Vulnerability Details  
   - Steps to Reproduce / PoC  
   - Impact / Severity  
   - Mitigation / Recommendations  
5. **Optional Tools Verification:** Use static analyzers (like Remix basic analyzer) to confirm findings.  

---

## Reports

| Level | Contract | Report |
|-------|----------|--------|
| 1 | Fallback | [Level 1 – Fallback Audit](./Level1_Fallback.md) |
| 2 | Fallback | [Level 2 – Fallout Audit](./Level2_Fallback_Audit.md) |
| 3 | … | … |
| … | … | … |

> Each report is a **medium-length audit**, with clear PoC steps and mitigation advice, ready for GitHub readers or as a reference for bounty-style reporting.

---

## Notes & Tips

- All audits are **for learning purposes**; do not use exploits on live contracts.  
- This repo demonstrates **manual auditing reasoning first**, then tool verification second.  
- Future updates will include **more Ethernaut levels and advanced contract audits**.

## License

All audit content is **MIT License**, feel free to study, reference, and learn.
