# Run `ollama` docker container

```shell
docker compose up --build ollama
```

## Pull SLMs

* Download SLM/LLM (one or more models, can be done one time only, the list below is intended for NVidia GTX-1650 (4Gb))

```shell
docker exec -it $(docker ps -qf "name=ollama") ollama pull qwen2.5-coder:7b-instruct-q3_K_M
```
Extra SLM/LLM models that can be used too

```shell
docker exec -it $(docker ps -qf "name=ollama") ollama pull llama3.2:3b-instruct-q4_K_M
docker exec -it $(docker ps -qf "name=ollama") ollama pull qwen2.5-coder:3b-instruct-q4_K_M
docker exec -it $(docker ps -qf "name=ollama") ollama pull phi3.5:latest
docker exec -it $(docker ps -qf "name=ollama") ollama pull qwen3.5:4b
```

## Check if SLM/LLM is available

## Get the list of models available

```shell
docker exec -it $(docker ps -qf "name=ollama") ollama list
```
## Communicate with the model desired in interractive mode

```shell
docker exec -it $(docker ps -qf "name=ollama") ollama run "qwen2.5-coder:7b-instruct-q3_K_M"
```

## Communicate with the model desired in batch mode

```shell
curl http://localhost:11434/v1/chat/completions \
    -H "Content-Type: application/json" \
    -d '{
        "model": "qwen2.5-coder:7b-instruct-q3_K_M",
        "max_tokens": 150,
        "messages": [{"role": "user", "content": "Who are you? Please introduce yourself briefly."}],
        "stream": false
    }'
```

*_NOTE:_* If you wanted to use a different model please use appropriate value for MODEL environment variable above

# Run `litellm` translating proxy

```shell
docker compose up --build litellm
```

## Check if translating proxy `litellm` can work with `ollama` placed models

```shell
curl http://localhost:4000/v1/messages \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer ollama" \
    -H "anthropic-version: 2023-06-01" \
    -d '{
        "model": "qwen2.5-coder:7b-instruct-q3_K_M",
        "max_tokens": 150,
        "messages": [{"role": "user", "content": "Who are you? Please introduce yourself briefly."}]
    }'
```

# Run Claude Code

## Install `claude code` (if it is required)

```shell
curl -fsSL https://claude.ai/install.sh | bash
```

## Run Claude Code application

```shell
ANTHROPIC_BASE_URL="http://localhost:4000" \
ANTHROPIC_MODEL="qwen2.5-coder:7b-instruct-q3_K_M" \
ANTHROPIC_DEFAULT_SONNET_MODEL="qwen2.5-coder:7b-instruct-q3_K_M" \
ANTHROPIC_DEFAULT_HAIKU_MODEL="qwen2.5-coder:7b-instruct-q3_K_M" \
ANTHROPIC_DEFAULT_OPUS_MODEL="qwen2.5-coder:7b-instruct-q3_K_M" \
ANTHROPIC_AUTH_TOKEN="ollama" \
ANTHROPIC_API_KEY="ollama" \
CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1 \
DISABLE_PROMPT_CACHING=1 \
DISABLE_INTERLEAVED_THINKING=1 \
claude
```
