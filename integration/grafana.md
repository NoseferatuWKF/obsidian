>Prometheus aggregates log via node exporter or push gateway (for containers that don't support prometheus) Loki collects all logs via promtail, Grafana fetch and visualize logs

## Setup

>default credential is admin:admin

nginx
```bash
proxy_set_header Host $host;
```

## Alerts

### Getting started

create contact point
>alerting > contact points > add contact point

![[Pasted image 20230410120138.png]]

create new notification policy
>alerting > notification policies > add policy 

>scroll down and click on edit after adding a new policy

![[Pasted image 20230410120516.png]]

create new alert rule
>alerting > alert rules > create alert rule

![[Pasted image 20230410120825.png]]

### Fine tuning

notification policy

alert rule

### [Alert Templating](https://grafana.com/docs/grafana/latest/alerting/manage-notifications/template-notifications/using-go-templating-language/)

[example](https://grafana.com/docs/grafana/latest/alerting/manage-notifications/template-notifications/create-notification-templates/)
[data and functions references](https://grafana.com/docs/grafana/latest/alerting/manage-notifications/template-notifications/reference/)

templates
```go
{{ define "print_labels" }}
{{ end }}
{{ template "print_labels" . }} // use template
{{ template "print_alerts" .Alerts }} // passing Alerts to template
```

print value
```go
{{ .Alerts }} // print all in alerts array
```

comments
```go
{{/* This is a comment */}}
```

range
```go
{{ range .Alerts }}
{{ .Labels }}
{{ end }}
```

index
```go
{{ range .Alerts }}
The name of the alert is {{ index .Labels "alertname" }}
{{ end }}
```

range with index
```go
{{ $num_alerts := len .Alerts }}
{{ range $index, $alert := .Alerts }}
This is alert {{ $index }} out of {{ $num_alerts }}
{{ end }}
```

if statements
```go
{{ if .Alerts }}
There are alerts
{{ else }}
There are no alerts
{{ end }}
```

with statements
```go
{{ with .Alerts }}
There are {{ len . }} alert(s)
{{ else }}
There are no alerts
{{ end }}
```

variables
```go
{{ range .Alerts }}
{{ $alert := . }}
{{ range .Labels.SortedPairs }}
{{ .Name }} = {{ .Value }}
{{/* works because $alert refers to the value of dot inside the first range */}}
There are {{ len $alert.Labels }}
{{ end }}
{{ end }}
```

remove spaces and line breaks
```go
{{ range .Alerts -}} // remove white spaces after
  {{ range .Labels.SortedPairs -}}
    {{ .Name }} = {{ .Value }}
  {{- end }} // remove white spaces before
{{- end -}} // remove white spaces before and after
```