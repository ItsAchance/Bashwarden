#!/bin/bash
BW_SESSION=$(bw unlock --raw)
items_json=$(bw list items --session "$BW_SESSION")

# Let user pick using fzf — show name + username, keep ID hidden but retrievable
selection=$(echo "$items_json" | jq -r '.[] | "\(.name) (\(.login.username // "no-username")) |\(.id)"' | fzf --with-nth=1 --delimiter="|" --prompt="🔐 Bitwarden: ")

[ -z "$selection" ] && exit 1
item_id=$(echo "$selection" | awk -F '|' '{print $2}')
password=$(bw get password "$item_id" --session "$BW_SESSION")

echo -n "$password" | pbcopy
osascript -e "display notification \"Password copied to clipboard.\" with title \"Bitwarden\""
(sleep 20 && echo -n "" | pbcopy && osascript -e "display notification \"Clipboard cleared.\" with title \"Bitwarden\"") &

