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
