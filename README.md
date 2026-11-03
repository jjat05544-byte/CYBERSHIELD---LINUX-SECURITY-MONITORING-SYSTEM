# CYBERSHIELD---LINUX-SECURITY-MONITORING-SYSTEM
access_audit_menu(){
while true; do
echo "--- USERS, GROUPS & PERMISSIONS AUDIT ---"
echo "1. Current user identity  2. Local user names  3. Current groups"
echo "4. Project permissions  5. Check ACL utilities  0. Back"
read -rp "Choose: " c
case "$c" in
1) whoami; id ;;
2) cut -d: -f1 /etc/passwd | sort | head -30 ;;
3) groups; getent group | head -20 ;;
4) ls -ld "$BASE_DIR" "$DATA_DIR" "$REPORT_DIR"; stat -c '%A %U:%G %n' "$BASE_DIR" "$DATA_DIR" "$REPORT_DIR" 2>/dev/null ;;
5) command -v setfacl; command -v getfacl ;;
0) break ;; *) echo "Invalid option" ;;
esac
done
}
log_analysis_menu(){
sample="$DATA_DIR/sample_security.log"
if [[ ! -f "$sample" ]]; then
cat > "$sample" <<'EOF'
2026-01-10 09:10:00 INFO User admin logged in
2026-01-10 09:15:00 WARNING Failed login for user guest
2026-01-10 09:16:00 WARNING Failed login for user guest
2026-01-10 09:20:00 INFO Backup completed
2026-01-10 09:25:00 ERROR Permission denied on secure folder
2026-01-10 09:30:00 INFO User operator logged out
2026-01-10 09:35:00 ERROR Authentication service unavailable
EOF
fi
while true; do
echo "--- SECURITY LOG ANALYSIS ---"
echo "1. View sample log  2. Search warnings  3. Search errors"
echo "4. Count event types  5. Extract date and event  6. Exact phrase search  0. Back"
read -rp "Choose: " c
case "$c" in
1) cat "$sample" ;;
2) grep -in 'warning' "$sample" ;;
3) grep -in 'error' "$sample" ;;
4) grep -oE 'INFO|WARNING|ERROR' "$sample" | sort | uniq -c ;;
5) cut -d' ' -f1,3- "$sample" ;;
6) read -rp "Phrase: " p; grep -Fni -- "$p" "$sample" || echo "No match" ;;
0) break ;; *) echo "Invalid option" ;;
esac
done
}
