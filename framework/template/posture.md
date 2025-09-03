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
{{- range $ruleResult := $finding.Results}}
Rule ID: {{$ruleResult.RuleId}}
{{- $hasFailure := false }}
{{- range $subj := $ruleResult.Subjects}}
{{- range $prop := $subj.Props}}
{{- if and (eq $prop.Name "result") (eq $prop.Value "fail") }}
{{- $hasFailure = true }}
{{- end}}
{{- end}}
{{- end}}

<details{{if $hasFailure}} open{{end}}>

<summary>{{if $hasFailure}}Failed Rule Details{{else}}Rule Details{{end}}</summary>
{{- range $subj := $ruleResult.Subjects}}

- Subject UUID: {{$subj.SubjectUuid}}
- Title: {{$subj.Title}}
{{- range $prop := $subj.Props}}
{{- if eq $prop.Name "result"}}
{{- if eq $prop.Value "fail"}}

  - **Result: {{$prop.Value}}**
{{- else if eq $prop.Value "pass"}}

  - **Result: {{$prop.Value}}**
{{- else}}

  - **Result: {{$prop.Value}}**
{{- end}}
{{- end}}

{{- if eq $prop.Name "reason"}}
    <details{{if $hasFailure}} open{{end}}>
    <summary>{{if $hasFailure}}Failure Reason{{else}}Details{{end}}</summary>
    - Reason:

      ```

      {{ newline_with_indent $prop.Value 6}}
      ```

    </details>

{{- end}}
{{- end}}
{{- end}}
  </details>
{{- end}}
{{- end}}
{{- end}}
{{- else}}

No Findings.
{{- end}}
{{- end}}
