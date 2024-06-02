# ABOUT
This repo is a simple demo of testing publishing helm chart version to Github pages with  [chart-releaser-action](https://github.com/helm/chart-releaser-action).

## Prerequisite
Same prerequisites as listed in [chart-releaser-action]( https://github.com/helm/chart-releaser-action?tab=readme-ov-file#pre-requisites).
1. A GitHub repo containing a directory with your Helm charts (default is a folder named `/charts`, if you want to maintain your charts in a different directory, you must include a `charts_dir` input in the workflow).
1. A GitHub branch called `gh-pages` to store the published charts.
1. In your repo, go to Settings/Pages. Change the `Source` `Branch` to `gh-pages`.
1. Create a workflow `.yml` file in your `.github/workflows` directory. An [example workflow](#example-workflow) is available below.

## How to release
Update chart detail in your main branch like [this commit](https://github.com/jianxing31/helm_chart_publish_test/commit/2d2e28499d65b0aae0c2105c28aa523dced651b5), Github actions will be triggered to package our helm charts and publish them to Github pages.
![image1](https://github.com/jianxing31/helm_chart_publish_test/blob/main/images/chart_update_test.jpg)
A sample of released helm chart.
![image1](https://github.com/jianxing31/helm_chart_publish_test/blob/main/images/chart_release.jpg)

## How to use released charts
A specific version can be referenced with version key in our configuration file. For example, with helmfile.yaml the setting will be similar to the following code.
```shell
environments:
  dev:
  prod:

repositories:
  - name: my-repo
    url: https://raw.githubusercontent.com/jianxing31/helm_chart_publish_test/gh-pages

releases:
  - name: my-repo-{{ .Environment.Name }}
    namespace: {{ .Environment.Name }}
    chart: my-repo/mlops
    version: 0.2.0
    values:
      - ./my_values.yaml
```