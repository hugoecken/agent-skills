# A standalone script stays small

A local CSV counter needs neither a service class, protocols, Pydantic Settings nor a package tree. This example uses only the standard library in production. It deliberately counts logical CSV records after one header; it is not a supplier ingestion pipeline or schema validator. A richer accepted use case may require those responsibilities later.

Save the two files together in an isolated directory. Run `python count_csv.py input.csv`; pytest exercises synthetic temporary files. The example passes five cases, Ruff lint/format and mypy strict for both files. Tests require pytest; installing the skill adds no dependency to an existing application.

## `count_csv.py`

```python
"""Count CSV data records without introducing an application architecture."""

import argparse
import csv
from collections.abc import Sequence
from pathlib import Path


def count_records(path: Path) -> int:
    """Count nonblank records after one header; own and close the UTF-8 file.

    Empty files contain zero records. csv.reader handles quoted multiline fields.
    This utility counts records; it does not validate a supplier schema.
    """
    with path.open(encoding="utf-8", newline="") as source:
        records = csv.reader(source, strict=True)
        next(records, None)
        return sum(1 for record in records if record)


def main(arguments: Sequence[str] | None = None) -> int:
    """Parse one local filename and report count or a bounded input diagnostic."""
    parser = argparse.ArgumentParser(description=__doc__)
    parser.add_argument("path", type=Path)
    options = parser.parse_args(arguments)
    path: Path = options.path
    try:
        count = count_records(path)
    except (OSError, UnicodeError, csv.Error):
        parser.error("Could not read a valid UTF-8 CSV file")
    print(count)
    return 0


if __name__ == "__main__":
    raise SystemExit(main())
```

## `test_count_csv.py`

```python
"""Literal file scenarios for the standalone counter."""

from pathlib import Path

import pytest

from count_csv import count_records, main


@pytest.mark.parametrize(
    ("content", "expected"),
    [("", 0), ("name\n", 0), ('name\n"two\nlines"\n\nother\n', 2)],
)
def test_counts_logical_data_records(
    tmp_path: Path, content: str, expected: int
) -> None:
    # Given
    path = tmp_path / "input.csv"
    path.write_text(content, encoding="utf-8")

    # When
    result = count_records(path)

    # Then
    assert result == expected


def test_cli_reports_count(tmp_path: Path, capsys: pytest.CaptureFixture[str]) -> None:
    # Given
    path = tmp_path / "input.csv"
    path.write_text("name\nA\n", encoding="utf-8")

    # When
    code = main([str(path)])

    # Then
    assert code == 0
    assert capsys.readouterr().out == "1\n"


def test_cli_rejects_malformed_csv(
    tmp_path: Path, capsys: pytest.CaptureFixture[str]
) -> None:
    # Given
    path = tmp_path / "input.csv"
    path.write_text('name\n"unterminated', encoding="utf-8")

    # When / Then
    with pytest.raises(SystemExit) as failure:
        main([str(path)])
    assert failure.value.code == 2
    assert "valid UTF-8 CSV" in capsys.readouterr().err
```
