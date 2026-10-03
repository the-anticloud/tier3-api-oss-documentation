# Developer Cookbook — api-oss-documentation
**Stack:** Python 3.11, mkdocs, pdoc3, PAX 27B, AIOSS_FORMAT
**Domain:** Sovereign auto-generated documentation for Anticloud projects
**License:** Apache-2.0 | **IP:** USPTO pending 2026, Anticloud FZ LLE

## Core Usage

```python
from api_oss_documentation import DocBuilder
builder = DocBuilder(source_dir='./TIER_4_INFERENCE_AGENTS/K_SGLANG', aioss_chain='./docs.aioss')
docs = builder.build(pax_model='./pax-27b-q4.gguf', output_dir='./docs/')
print(f'Coverage: {docs.coverage:.1%}, Chain: {docs.chain_hash}')
```

## AIOSS Chain Append

```python
import hashlib, time

def aioss_append(chain_path, payload: bytes, module_id: str):
    entry_hash = hashlib.sha3_256(payload).digest()
    ts = int(time.time_ns()).to_bytes(8, 'big')
    with open(chain_path, 'rb') as f:
        f.seek(-32, 2); prev_hash = f.read(32)
    new_hash = hashlib.sha3_256(prev_hash + entry_hash + ts).digest()
    with open(chain_path, 'ab') as f:
        f.write(ts + entry_hash + new_hash)
    return new_hash.hex()

# After every api-oss-documentation output:
chain_hash = aioss_append("./api_oss_documentation.aioss",
                           result_bytes, "api-oss-documentation")
```

## Performance & Integration

Performance: profile with api-oss-devtools. Benchmark with api-oss-analytics. Integration: all api-oss-documentation operations are logged to api-oss-logging and audited by api-oss-compliance.
