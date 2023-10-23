# Cross-References (X-Refs)

## Cross-References (X-Refs)

`axt` - Find X-Refs **to** a function `afx` - Analyze X-Refs **from** a function `axf` - `axff` - Find X-Refs **from** a function

{% tabs %}
{% tab title="afx" %}
```nasm
[0x08049050]> afx
c 0x08049050 -> 0x0804c010 jmp dword [reloc.fflush]
```
{% endtab %}

{% tab title="axf" %}
```nasm
[0x08049050]> axf
CODE 0x804c010 reloc.fflush
```
{% endtab %}

{% tab title="axff" %}
```nasm
[0x08049050]> axff
CODE 0x08049050 0x0804c010 reloc.fflush
```
{% endtab %}
{% endtabs %}