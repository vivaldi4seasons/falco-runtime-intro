# Rule: Memory dump process in Container [custom]


    - list: memory_dump_binaries
    items: [coredumpctl, gcore, memdump, dumpe2fs, dotnet-dump]

    - macro: memory_dump_procs
    condition: proc.name in (memory_dump_binaries) or (proc.cmdline startswith "dd if=/" or proc.cmdline startswith "mv core." or proc.cmdline startswith "strings core.")

    - rule: Memory dump process in Container [custom]
    desc: Detects processes initiating memory dumps inside a container
    condition: >
        spawned_process
        and container
        and memory_dump_procs
    enabled: false
    output: >
        Memory dump process launched in container (user=%user.name user_loginuid=%user.loginuid
        command=%proc.cmdline container_id=%container.id container_name=%container.name image=%container.image.repository:%container.image.tag)
    priority: NOTICE
    tags: [host, container, process, mitre_credential_access]

Se puede observar que se levanta la relga en Falco.

    {"hostname":"falcox33-falco-obsec-5z652","output":"22:39:47.261363951: Notice Memory dump process launched in container (user=<NA> user_loginuid=-1 command=dd if=/dev/mem of=mem_dump.bin bs=1M count=100 container_id=28c0c7a20575 container_name=ubuntu-lab image=docker.io/diegoall1990/ubuntu-lab:1.0.0) k8s.ns=falco-custom-lab k8s.pod=ubuntu-lab container=28c0c7a20575","priority":"Notice","rule":"Memory dump process in Container [custom]","source":"syscall","tags":["container","host","mitre_credential_access","process"],"time":"2025-02-26T22:39:47.261363951Z", "output_fields": {"container.id":"28c0c7a20575","container.image.repository":"docker.io/diegoall1990/ubuntu-lab","container.image.tag":"1.0.0","container.name":"ubuntu-lab","evt.time":1740609587261363951,"k8s.ns.name":"falco-custom-lab","k8s.pod.name":"ubuntu-lab","proc.cmdline":"dd if=/dev/mem of=mem_dump.bin bs=1M count=100","user.loginuid":-1,"user.name":"<NA>"}}