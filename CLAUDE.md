# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

LLM4DBdesign is a multi-agent framework for automated database schema generation using large language models. The system converts natural language requirements into normalized database schemas through conceptual modeling and logical design phases.

## Development Commands

### Running the main system
```bash
python run.py --model_name gpt4 --dataset_name DBexample --method domain_analyse
```

### Key command-line arguments:
- `--model_name`: Model to use (gpt4, chatgpt, deepseek, glm4)
- `--method`: Generation method (base_direct, base_cot, domain_analyse, expert_analyse, pseudo_code_analyse)
- `--dataset_dir`: Path to dataset (default: ./datasets/RSchema/)
- `--verification`: Verification methods for quality control
- `--start_pos` / `--end_pos`: Test range for batch processing

### Installing dependencies
```bash
pip install -r requirements.txt
```

## Architecture

### Core Components

1. **Multi-Agent Framework**: Two implementation approaches:
   - `agent_format.py`: Custom multi-agent implementation with specialized agents for different DB design phases
   - `AutoGen_based/`: Alternative implementation using Microsoft AutoGen framework

2. **Database Design Pipeline**:
   - **Phase 1**: Conceptual modeling (requirements → entities, relationships, attributes)
   - **Phase 2**: Logical modeling (normalization, schema generation)
   - **Phase 3**: Quality verification through iterative agent consensus

3. **Agent Specialization**:
   - Entity identification agents
   - Relationship analysis agents  
   - Functional dependency agents
   - Primary key detection agents
   - Quality control/verification agents

### Key Modules

- `run.py`: Main entry point with argument parsing and execution flow
- `agent_format.py`: Core multi-agent orchestration logic
- `prompt_generator_format.py`: Prompt templates for different agents
- `utils.py`: Utility functions for functional dependency analysis and normalization
- `data_utils.py`: Dataset loading and management
- `api_utils.py`: LLM API interaction handling

### Data Flow

1. Load requirements from `datasets/RSchema/annotation.jsonl`
2. Apply selected method (direct, CoT, or multi-agent approaches)
3. For multi-agent methods:
   - Entity/relationship identification
   - Functional dependency analysis
   - Armstrong axiom application for key detection
   - 3NF normalization and table decomposition
   - Quality verification through agent consensus
4. Output generated schemas to `outputs/DBdesign_domain/`

### Configuration

- API keys and endpoints configured in `api_utils.py`
- Dataset paths and model selection via command-line arguments
- Quality control parameters (verification types, retry attempts) configurable

### Notable Design Patterns

- **Multi-round verification**: Agents can revise outputs based on quality controller feedback
- **Algorithmic validation**: Armstrong axioms and normalization algorithms complement LLM reasoning
- **Modular agent design**: Each design phase handled by specialized agents with domain-specific prompts