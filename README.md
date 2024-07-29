Redistribution of the standard `text/template` Go package with the
following modifications:

## Added the `~` and `#` space trimmers:

`~` will only trim whitespace characters that do not represent a new
line (CR or LF).

```
line 1
  {{~ "line 2"}}
line 3

=>
line 1
line 2
line 3
```

`#` will do:

- If the trimmer is on the left, it will seek whitespace until it
finds something that is not whitespace. Then, if it was on a different
line from where it started, it will leave that line's line break
behind and consume the rest. If there was trailing whitespace in that
line, it will be kept too.

- If the trimmer is on the right, it will seek whitespace until it
finds something that is not whitespace. Then, if it was on a different
line from where it started, it will leave that line's whitespace behind
and consume the rest.

```
  line 1

  {{# "line 2"}}
  line 3

=>
  line 1
line 2
  line 3
```

```
  line 1
  {{"line 2" #}}

  line 3

=>
  line 1
  line 2  line 3
```

## Made {{template}} allow terms as template names

```
{{- define "one" -}} I {{.}} {{- end -}}
{{- $tmpl := "one" -}}

{{template $tmpl "u"}}

=>
I u
```
