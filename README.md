# gh-actions-template

テンプレート用リポジトリ（GitHub Actions の再利用ワークフロー／composite action を収める）です。

目的:
- 複数リポジトリで共通の CI/CD ステップ（Terraform の Plan/Apply、ツールのインストールなど）を再利用する。

 - `.github/workflows/terraform-plan.yml` — 再利用ワークフロー（`workflow_call`）: 指定した `working_dir` に対して `terraform plan` を実行します。`target` 入力を与えない場合は、`working_dir` の basename を `tfcmt` の `target` として使います。
 - `.github/workflows/terraform-apply.yml` — 再利用ワークフロー（`workflow_call`）: 指定した `working_dir` に対して `terraform apply` を実行します。`target` 入力を与えない場合は、`working_dir` の basename を `tfcmt` の `target` として使います。
- `.github/actions/install-tools` — composite action: `aqua` の導入と `aqua install` を行います。

使い方（呼び出し側ワークフローの例）:

```
jobs:
  call-plan:
    uses: k-ngs/gh-actions-template/.github/workflows/terraform-plan.yml@v1
    with:
      working_dir: environments/dev
      aqua_version: v2.28.0
    secrets:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

バージョニング:
- 呼び出し側はタグ（例: `@v1`）で参照してください。互換性のある変更はメジャーを上げる形で公開します。

カスタマイズ:
- `terraform-version` を別ファイルに置いている場合は、ワークフロー内の `Terraform Version` ステップを調整してください。

次のアクション候補:
- 現在のリポジトリ（`tf-gh-actions`）のワークフローをこのテンプレートを呼ぶ形に置き換える。
- `install-tools` の入れ物に `tfcmt` 固有の設定やオプションを追加する。
