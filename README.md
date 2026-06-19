# AWS Certified Solutions Architect – Associate (SAA-C03) Practice Questions

100 original scenario-based practice questions written from scratch to help you prepare for the **AWS Certified Solutions Architect – Associate (SAA-C03)** exam. Every question, answer, and explanation in this repository is original content — written based on publicly available AWS service documentation and the official SAA-C03 exam guide domains, not copied or adapted from any existing question bank, exam dump, or third-party material.

## Why this repo exists

Most "exam dump" repos circulating online reproduce actual or near-actual exam questions, which violates the AWS Certification Agreement that every candidate agrees to when registering for an exam. This repo takes a different approach: scenario-style questions and hands-on labs written independently, covering the same domains and services the real exam tests, without reproducing anyone else's proprietary or leaked content — and without depending on any third-party lab platform. Everything here runs in your own AWS account.

## Structure

Questions are grouped into the four official SAA-C03 exam domains, weighted roughly in proportion to the [official exam guide](https://aws.amazon.com/certification/certified-solutions-architect-associate/):

| File | Domain | Exam Weight | # Questions |
|---|---|---|---|
| [`domain-1-secure-architectures.md`](./domain-1-secure-architectures.md) | Design Secure Architectures | 30% | 30 |
| [`domain-2-resilient-architectures.md`](./domain-2-resilient-architectures.md) | Design Resilient Architectures | 26% | 26 |
| [`domain-3-high-performing-architectures.md`](./domain-3-high-performing-architectures.md) | Design High-Performing Architectures | 24% | 24 |
| [`domain-4-cost-optimized-architectures.md`](./domain-4-cost-optimized-architectures.md) | Design Cost-Optimized Architectures | 20% | 20 |

Each question is a multiple-choice scenario with four options. The correct answer and a short explanation are hidden in a collapsible section directly underneath — try to answer first, then expand to check yourself.

## Hands-on labs

Multiple-choice questions only get you so far — the real exam (and the real job) tests whether you've actually clicked the buttons. The companion repo **[devish2/AWS-SAA-C-03-Exam-Practice-Lab](https://github.com/devish2/AWS-SAA-C-03-Exam-Practice-Lab)** has 24 original, self-contained hands-on labs (6 per domain) that you run directly in your own AWS account, each one cross-referenced back to the specific practice questions here, with validation steps and explicit cleanup instructions so nothing keeps billing you.

Start here: [`00-prerequisites-and-setup.md`](https://github.com/devish2/AWS-SAA-C-03-Exam-Practice-Lab/blob/main/00-prerequisites-and-setup.md) in the labs repo.

| Lab file (in the labs repo) | Maps to (in this repo) |
|---|---|
| [`domain-1-secure-labs.md`](https://github.com/devish2/AWS-SAA-C-03-Exam-Practice-Lab/blob/main/domain-1-secure-labs.md) | [`domain-1-secure-architectures.md`](./domain-1-secure-architectures.md) |
| [`domain-2-resilient-labs.md`](https://github.com/devish2/AWS-SAA-C-03-Exam-Practice-Lab/blob/main/domain-2-resilient-labs.md) | [`domain-2-resilient-architectures.md`](./domain-2-resilient-architectures.md) |
| [`domain-3-high-performing-labs.md`](https://github.com/devish2/AWS-SAA-C-03-Exam-Practice-Lab/blob/main/domain-3-high-performing-labs.md) | [`domain-3-high-performing-architectures.md`](./domain-3-high-performing-architectures.md) |
| [`domain-4-cost-optimized-labs.md`](https://github.com/devish2/AWS-SAA-C-03-Exam-Practice-Lab/blob/main/domain-4-cost-optimized-labs.md) | [`domain-4-cost-optimized-architectures.md`](./domain-4-cost-optimized-architectures.md) |

## How to use this

1. Go through each domain's question file and answer before expanding the solution.
2. Don't just memorize the answer — read the explanation and understand *why* the wrong options are wrong. The real exam tests judgment between several technically-plausible-sounding answers, not recall of a single fact.
3. Pair each domain's questions with its matching lab in the [labs repo](https://github.com/devish2/AWS-SAA-C-03-Exam-Practice-Lab). Do the lab, then go back and re-answer the questions it maps to — you'll retain it far better than reading alone.
4. **Set the zero-spend budget alarm** (first lab in the labs repo's setup guide) before doing anything else.

## Important considerations

- AWS services change. If something here looks outdated, please open an issue or PR.
- This is a study supplement, not a guarantee of passing the exam.
- These questions are original works and are not derived from real exam content, in compliance with the AWS Certification Agreement.

## Contributing

Found an error, an outdated service detail, or want to add more questions in the same original style? PRs welcome.

## License

MIT — use freely, just don't misrepresent it as official AWS material.
