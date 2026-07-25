## Financial Statement Extraction from PDF Reports Using LLM Structured Output

This project mirrors a similar tool I built at work — one that extracts structured financial data from research documents into a human-review UI — which couldn't be open-sourced due to confidentiality. This is a parallel implementation using public company financial statements instead.

Built a tool that extracts key financial metrics from Taiwan-listed companies' quarterly reports — converting scanned PDF income statements into structured JSON/CSV, with Claude's structured output enforcing a strict schema so extraction results are consistent and machine-readable.

![Upload interface for the financial statement extractor](docs/fs-extractor/upload.png)

A key design decision was separating what the LLM extracts from what gets calculated. Rather than asking the model to compute ratios like gross margin or operating margin — which risks silent rounding errors or fabricated numbers — the LLM only extracts raw figures printed on the statement (revenue, cost, gross profit, operating income, net income, EPS). All derived ratios and cross-checks are computed deterministically in a separate validation layer.

![Extraction result showing raw figures alongside computed ratios and validation status](docs/fs-extractor/result.png)

That validation layer surfaces a subtlety specific to Taiwan's consolidated financial statements: companies with intra-group transactions report a pre-adjustment gross profit line and a separate net gross profit line after eliminating unrealized intercompany gains. Reconstructing gross profit as simply revenue minus cost only holds exactly against the pre-adjustment figure — treating it as a hard equality against the reported (post-adjustment) number produces false-positive warnings for companies with subsidiary structures. The validation layer distinguishes "arithmetic must hold exactly" from "a real accounting adjustment exists here," rather than collapsing both into one fuzzy tolerance threshold.

The LLM provider itself is abstracted behind a common interface, so switching between Claude and Gemini is a single environment variable change rather than a rewrite.
