# Provedex

We test vLLM and SGLang on RTX PRO 6000 and PCIe multi-GPU servers, fix what breaks upstream, and publish the
numbers.

Every run lives in [sm120-kit](https://github.com/adi-suresh01/sm120-kit): the repro harness, one kit per issue, and
the results with their setup. The results table is on [provedex.io](https://provedex.io).

## Upstream

Merged:

- [sglang#44480](https://github.com/sgl-project/sglang/pull/44480): fixes an expert-parallel MoE crash on 2x PCIe
  RTX PRO 6000.
- [Puget Systems app packs #12](https://github.com/Puget-Systems/puget-docker-app-packs/pull/12) and
  [#15](https://github.com/Puget-Systems/puget-docker-app-packs/pull/15): CUDA graphs back on for seven Team LLM pack
  models, up to 9.1x single-user throughput.

Open:

- [vllm#62110](https://github.com/vllm-project/vllm/pull/62110): evicts loaded shards, so a 475 GiB DeepSeek V4.1
  checkpoint loads at a 291 GiB host-memory peak instead of 695 GiB and climbing.
- [sglang#44448](https://github.com/sgl-project/sglang/pull/44448): the custom all-reduce stages VMM graph inputs
  instead of IPC-registering them.

## Measured

- DeepSeek-V4.1-Flash at TP8 on 8x RTX PRO 6000 (vLLM): FlashInfer's PCIe IPC all-reduce cut single-user decode ITL
  20% with no prefill cost. [Kit and numbers](https://github.com/adi-suresh01/sm120-kit/tree/main/issues/vllm-59203)
- Does NCCL use P2P on your box? A two-command check:
  [pcie-tp8-allreduce](https://github.com/adi-suresh01/sm120-kit/tree/main/issues/pcie-tp8-allreduce#check-your-own-box)

Contact: hello@provedex.io

## Previous product

Provedex started as a signed audit log for AI agents (Ed25519-signed, SHA-256-chained, verifiable offline). Those
repos are archived and read-only: [provedex](https://github.com/provedex/provedex),
[compliance-backend-rfc](https://github.com/provedex/compliance-backend-rfc),
[demo-voice](https://github.com/provedex/demo-voice).
