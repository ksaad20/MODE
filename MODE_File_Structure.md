```

MODE/
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.yml
│   │   ├── feature_request.yml
│   │   └── config.yml
│   ├── workflows/
│   │   ├── ci.yml
│   │   ├── security.yml
│   │   ├── release.yml
│   │   └── dependency-review.yml
│   ├── dependabot.yml
│   └── CODEOWNERS
│
├── .devcontainer/
│   ├── devcontainer.json
│   └── Dockerfile
│
├── .gitignore
├── .gitattributes
├── .editorconfig
├── .pre-commit-config.yaml
├── .dockerignore
├── .python-version
│
├── Dockerfile
├── docker-compose.yml
├── Makefile
├── LICENSE
├── README.md
├── CHANGELOG.md
├── CONTRIBUTING.md
├── SECURITY.md
├── CODE_OF_CONDUCT.md
├── pyproject.toml
│
├── configs/
│   ├── default.yaml
│   ├── development.yaml
│   ├── production.yaml
│   └── examples/
│       ├── security_assessment.yaml
│       └── saas_example.yaml
│
├── docs/
│   ├── architecture/
│   │   ├── overview.md
│   │   ├── data_flow.md
│   │   ├── evidence_graph.md
│   │   ├── scoring.md
│   │   └── provider_architecture.md
│   │
│   ├── concepts/
│   │   ├── offers.md
│   │   ├── markets.md
│   │   ├── signals.md
│   │   ├── evidence.md
│   │   ├── opportunities.md
│   │   ├── timing.md
│   │   └── outcomes.md
│   │
│   ├── guides/
│   │   ├── installation.md
│   │   ├── configuration.md
│   │   ├── first-discovery.md
│   │   ├── custom-scorers.md
│   │   └── provider-integration.md
│   │
│   └── api/
│       └── openapi.yaml
│
├── examples/
│   ├── basic_discovery/
│   │   ├── config.yaml
│   │   └── README.md
│   ├── custom_signal/
│   │   ├── signal.py
│   │   └── config.yaml
│   └── custom_provider/
│       ├── provider.py
│       └── config.yaml
│
├── scripts/
│   ├── bootstrap.py
│   ├── validate_config.py
│   ├── seed_database.py
│   └── release.py
│
├── migrations/
│   └── ...
│
├── src/
│   └── mode/
│       │
│       ├── __init__.py
│       ├── __main__.py
│       │
│       ├── core/
│       │   ├── __init__.py
│       │   ├── config.py
│       │   ├── exceptions.py
│       │   ├── logging.py
│       │   ├── lifecycle.py
│       │   └── version.py
│       │
│       ├── domain/
│       │   ├── __init__.py
│       │   │
│       │   ├── offer/
│       │   │   ├── __init__.py
│       │   │   ├── models.py
│       │   │   ├── compiler.py
│       │   │   └── validator.py
│       │   │
│       │   ├── market/
│       │   │   ├── __init__.py
│       │   │   ├── models.py
│       │   │   ├── hypotheses.py
│       │   │   └── segmentation.py
│       │   │
│       │   ├── signal/
│       │   │   ├── __init__.py
│       │   │   ├── models.py
│       │   │   ├── registry.py
│       │   │   └── evaluator.py
│       │   │
│       │   ├── evidence/
│       │   │   ├── __init__.py
│       │   │   ├── models.py
│       │   │   ├── provenance.py
│       │   │   ├── confidence.py
│       │   │   └── graph.py
│       │   │
│       │   ├── organization/
│       │   │   ├── __init__.py
│       │   │   ├── models.py
│       │   │   └── resolver.py
│       │   │
│       │   ├── person/
│       │   │   ├── __init__.py
│       │   │   ├── models.py
│       │   │   └── resolver.py
│       │   │
│       │   ├── opportunity/
│       │   │   ├── __init__.py
│       │   │   ├── models.py
│       │   │   ├── evaluator.py
│       │   │   ├── timing.py
│       │   │   └── prioritizer.py
│       │   │
│       │   └── outcome/
│       │       ├── __init__.py
│       │       ├── models.py
│       │       └── taxonomy.py
│       │
│       ├── discovery/
│       │   ├── __init__.py
│       │   ├── engine.py
│       │   ├── pipeline.py
│       │   └── providers/
│       │       ├── __init__.py
│       │       ├── base.py
│       │       ├── registry.py
│       │       └── adapters/
│       │           └── ...
│       │
│       ├── enrichment/
│       │   ├── __init__.py
│       │   ├── engine.py
│       │   ├── pipeline.py
│       │   └── providers/
│       │       └── ...
│       │
│       ├── intelligence/
│       │   ├── __init__.py
│       │   ├── market_analyzer.py
│       │   ├── opportunity_engine.py
│       │   ├── evidence_engine.py
│       │   ├── timing_engine.py
│       │   └── buyer_engine.py
│       │
│       ├── scoring/
│       │   ├── __init__.py
│       │   ├── base.py
│       │   ├── registry.py
│       │   ├── fit.py
│       │   ├── intent.py
│       │   ├── timing.py
│       │   ├── confidence.py
│       │   └── composite.py
│       │
│       ├── recommendations/
│       │   ├── __init__.py
│       │   ├── engine.py
│       │   ├── actions.py
│       │   └── templates.py
│       │
│       ├── learning/
│       │   ├── __init__.py
│       │   ├── feedback.py
│       │   ├── calibration.py
│       │   ├── evaluation.py
│       │   └── models.py
│       │
│       ├── storage/
│       │   ├── __init__.py
│       │   ├── repositories/
│       │   │   ├── offers.py
│       │   │   ├── organizations.py
│       │   │   ├── evidence.py
│       │   │   ├── opportunities.py
│       │   │   └── outcomes.py
│       │   ├── models/
│       │   │   └── ...
│       │   └── unit_of_work.py
│       │
│       ├── integrations/
│       │   ├── __init__.py
│       │   ├── http/
│       │   ├── crm/
│       │   ├── notifications/
│       │   └── exports/
│       │
│       ├── api/
│       │   ├── __init__.py
│       │   ├── app.py
│       │   ├── dependencies.py
│       │   ├── middleware.py
│       │   ├── errors.py
│       │   └── routes/
│       │       ├── health.py
│       │       ├── offers.py
│       │       ├── markets.py
│       │       ├── discovery.py
│       │       ├── opportunities.py
│       │       ├── evidence.py
│       │       └── outcomes.py
│       │
│       ├── cli/
│       │   ├── __init__.py
│       │   ├── main.py
│       │   └── commands/
│       │       ├── config.py
│       │       ├── offer.py
│       │       ├── discover.py
│       │       ├── rank.py
│       │       ├── export.py
│       │       └── evaluate.py
│       │
│       └── exporters/
│           ├── __init__.py
│           ├── json.py
│           ├── csv.py
│           └── parquet.py
│
├── tests/
│   ├── unit/
│   │   ├── domain/
│   │   ├── intelligence/
│   │   ├── scoring/
│   │   └── recommendations/
│   │
│   ├── integration/
│   │   ├── discovery/
│   │   ├── enrichment/
│   │   ├── storage/
│   │   └── api/
│   │
│   ├── contract/
│   │   └── providers/
│   │
│   ├── regression/
│   │   └── ...
│   │
│   ├── fixtures/
│   │   ├── organizations/
│   │   ├── evidence/
│   │   ├── opportunities/
│   │   └── signals/
│   │
│   └── conftest.py
│
└── data/
    ├── .gitkeep
    ├── raw/
    ├── processed/
    └── fixtures/
