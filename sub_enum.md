*** crt.sh *** 
```bash
#!/bin/bash
curl -s https://crt.sh/\?q\=\%.$1\&output\=json | jq -r '.[].name_value' | sed 's/\*\.//g' | sort -u
```

```bash
// put the domains you want to enumerate in the file wildcards like
// cat wildcards
// domain.com
// domain2.com 
//
//
cat wildcards | subfinder -all -o sf_output.txt
cat wildcards | assetfinder --subs-only >> af_output.txt
for i in $(cat wildcards);do findomain -t $i -u fd_output.txt;done
cat wildcards | gauplus -t 5 -random-agent | unfurl -u domains | anew gauplus.txt 
cat wildcards | waybackurls | unfurl -u domains | anew wayback_output.txt
for i in $(cat wildcards);do /opt/crt.sh $i >> crt.txt;done
curl https://raw.githubusercontent.com/proabiral/Fresh-Resolvers/master/resolvers.txt -o /opt/resolvers.txt;for i in $(cat wildcards);do puredns bruteforce /opt/assetnote.txt $i -r /opt/resolvers.txt -w `echo $i | cut -d "." -f1`.txt;done
for i in $(cat wildcards);do amass enum -passive -d $i -o  `echo $i | cut -d "." -f1`.amass.txt;done
cat * | unfurl domains | grep -i "$(cat wildcards| sed -z 's/\n/\\|/g;s/,$/\n/' | sed 's/\(.*\)\\|/\1/')" | sort -u > subs
mksub -df subs -l 1 -w /opt/permuts.txt -r "^[a-zA-Z0-9\.-_]+$" -o permuts.txt
gotator -sub subs -perm /opt/permuts.txt -depth 1 -numbers 10 -mindup -adv -md >> permuts.txt
puredns resolve permuts.txt -r /opt/resolvers.txt -w permutations.txt
rm permuts.txt
cat permutations.txt subs | grep -i "$(cat wildcards| sed -z 's/\n/\\|/g;s/,$/\n/' | sed 's/\(.*\)\\|/\1/')" | sort -u > subs2;mv subs2 subs
cat subs | naabu -top-ports 1000 -o naabu_portscan.txt | httpx -random-agent -retries 2 -no-color -o httpx.txt
gospider -S probed_tmp_scrap.txt --js -t 50 -d 3 --sitemap --robots -w -r > gospider.txt
cat gospider.txt | sed '/^.\{2048\}./d' > gospiderclean.txt;cat gospiderclean.txt | grep -Eo 'http?://[^ ]+' | sed 's/]$//' | unfurl -u domains | grep -i "$(cat wildcards| sed -z 's/\n/\\|/g;s/,$/\n/' | sed 's/\(.*\)\\|/\1/')" | sort -u > scrap_subs.txt
cat subs scrap_subs.txt | sort -u > subs2;mv subs2 subs
cat subs | naabu -top-ports 1000 | httpx -random-agent -retries 2 -no-color -o httpx.txt
```