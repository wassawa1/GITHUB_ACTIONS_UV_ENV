# Description
ローカルでGithub Actionsを実行するためのツール```act```の導入手順を説明する。

## motivation
Github Actionsを使っていると、コードにリファクタリングの余地がない場合であっても、ワークフローの記述が不適切で、意図した通りに動作しないことがしばしばある。

* 課題: **Commitログが汚くなる**
だんだんCommitメッセージが雑になっていく
→Pushする前にローカルでworkflowを実行できるようにできないか？
→actを使ってみようと思った


# Usage

* install

```bash
curl https://raw.githubusercontent.com/nektos/act/master/install.sh | sudo bash
```

* 使用リポジトリ

https://github.com/wassawa1/TEST_Github_Action/actions


* 実行スクリプト

```bash
./bin/act -l
./bin/act workflow_dispatch -W .github/workflows/<workflow>.yml
```




* 今回の例
<!-- ```bash
./bin/act -l
./bin/act workflow_dispatch -W .github/workflows/test.yml
``` -->

* 確認
```
/dev/null && echo OK || echo NG
```
```
WARNING: No blkio throttle.read_bps_device support
WARNING: No blkio throttle.write_bps_device support
WARNING: No blkio throttle.read_iops_device support
WARNING: No blkio throttle.write_iops_device support
OK
```

OK判定が出たので次。

* 利用可能なワークフロー一覧を表示

```bash
./bin/act -P ubuntu-latest=ghcr.io/catthehacker/ubuntu:full-24.04 -l
```
```
INFO[0000] Using docker host 'unix:///var/run/docker.sock', and daemon socket 'unix:///var/run/docker.sock' 
Stage  Job ID      Job name           Workflow name        Workflow file        Events
0      test_jobs   test_jobs          Python Package Test  remote-workflow.yml  push,workflow_dispatch
```
* ワークフローを実行

```bash
./bin/act workflow_dispatch -W .github/workflows/remote-workflow.yml \
  -P ubuntu-latest=ghcr.io/catthehacker/ubuntu:full-24.04
```

## ローカル実行のための Python 仮想環境（uv）
このリポジトリでは仮想環境の作成スクリプトを含めず、最小限のコマンドのみ README に記載します。ローカルでテストを実行するには、任意の方法で仮想環境を作成して `requirements.txt` をインストールしてください。

例（WSL / bash）:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip setuptools wheel
# Install editable package with dev extras from pyproject.toml
python -m pip install -e '.[dev]'
pytest
```

例（PowerShell）:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip setuptools wheel
python -m pip install -e '.[dev]'
pytest
```