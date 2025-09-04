# Assessment Results Details

## Catalog

{{.Catalog}}
{{- range $component := .Components}}

### Component: {{$component.ComponentTitle}}

{{- if $component.Findings }}
{{- range $finding := $component.Findings}}

-------------------------------------------------------

#### Result of control: {{$finding.ControlID}}

{{ if $finding.Results }}
{{- $hasFailedRules := false }}
{{- $hasPassedRules := false }}

{{- range $ruleResult := $finding.Results}}
{{- $hasFailure := false }}
{{- range $subj := $ruleResult.Subjects}}
{{- range $prop := $subj.Props}}
{{- if and (eq $prop.Name "result") (eq $prop.Value "fail") }}
{{- $hasFailure = true }}
{{- end}}
{{- end}}
{{- end}}
{{- if $hasFailure}}
{{- $hasFailedRules = true }}
{{- else}}
{{- $hasPassedRules = true }}
{{- end}}
{{- end}}

{{- if $hasFailedRules}}
<details open>
<summary> Failed Rules</summary>

{{- range $ruleResult := $finding.Results}}
{{- $hasFailure := false }}
{{- range $subj := $ruleResult.Subjects}}
{{- range $prop := $subj.Props}}
{{- if and (eq $prop.Name "result") (eq $prop.Value "fail") }}
{{- $hasFailure = true }}
{{- end}}
{{- end}}
{{- end}}
{{- if $hasFailure}}

**Rule ID:** {{$ruleResult.RuleId}}

<details open>
<summary>Failed Rule Details</summary>
{{- range $subj := $ruleResult.Subjects}}

- **Subject UUID:** {{$subj.SubjectUuid}}
- **Title:** {{$subj.Title}}
{{- range $prop := $subj.Props}}
{{- if eq $prop.Name "result"}}

  - **Result: {{$prop.Value}}**
{{- end}}

{{- if eq $prop.Name "reason"}}
    <details open>
    <summary>Failure Reason</summary>

    ```
    {{ newline_with_indent $prop.Value 4}}
    ```

    </details>
{{- end}}
{{- end}}
{{- end}}
</details>
{{- end}}
{{- end}}
</details>
{{- end}}

{{- if $hasPassedRules}}
<details>
<summary> Passed Rules</summary>

{{- range $ruleResult := $finding.Results}}
{{- $hasFailure := false }}
{{- range $subj := $ruleResult.Subjects}}
{{- range $prop := $subj.Props}}
{{- if and (eq $prop.Name "result") (eq $prop.Value "fail") }}
{{- $hasFailure = true }}
{{- end}}
{{- end}}
{{- end}}
{{- if not $hasFailure}}

**Rule ID:** {{$ruleResult.RuleId}}

<details>
<summary>Passed Rule Details</summary>
{{- range $subj := $ruleResult.Subjects}}

- **Subject UUID:** {{$subj.SubjectUuid}}
- **Title:** {{$subj.Title}}
{{- range $prop := $subj.Props}}
{{- if eq $prop.Name "result"}}

  - **Result: {{$prop.Value}}**
{{- end}}

{{- if eq $prop.Name "reason"}}
    <details>
    <summary>Details</summary>

    ```
    {{ newline_with_indent $prop.Value 4}}
    ```

    </details>
{{- end}}
{{- end}}
{{- end}}
</details>
{{- end}}
{{- end}}
</details>
{{- end}}
{{- end}}
{{- else}}

No Findings.
{{- end}}
{{- end}}
{{- end}}