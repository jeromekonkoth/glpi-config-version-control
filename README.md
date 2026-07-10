#Git: Best Practices for version controling

--------------------------------Initial_Push_GitLab_Test-serv----------------------------------------------
#GLPI_test_server
git init 
nano .gitignore  #Add the .gitignore info inside it 
git config --global --add safe.directory /var/www/glpi 
git init --initial-branch=main  
git remote -v 
git remote add origin git@192.168.150.139:jero/glpi.git 
sudo git add .  
git status 
sudo git commit -m "Initial import from test server" 
sudo git branch -m main staging  
sudo git push --set-upstream origin staging 
--------------------------------Update the 11.0.7 ---------------------------------------------
sudo systemctl stop apache2 
cd /var/www/glpi-11.0.7 
sudo tar -xvzf glpi-11.0.7.tgz 
sudo rm -r glpi-11.0.6.tgz 
diff -rq glpi/ glpi-11.0.7/glpi 
cd /var/www 
sudo rsync -avc --delete \ 
  --exclude 'config/' \ 
  --exclude 'files/' \ 
  --exclude '.git' \ 
  --exclude '.gitignore' \ 
 --exclude 'plugins/' \ 
 --exclude 'marketplace/' \ 
glpi-11.0.7/glpi/ glpi/ 
sudo chown -R www-data:www-data /var/www/glpi 
cd /var/www/glpi  
sudo -u www-data php bin/console db:update 
sudo systemctl start apache2 
--------------------------------Update_Push_glpi_11.0.7_GitLab---------------------------------------------
git add .
git status
git commit -m "Upgrade GLPI to 11.0.7"
git push origin staging
git branch
git checkout main
git pull origin main
git merge staging
git push origin main
git checkout staging
git merge main
git push origin staging
--------------------------------Merge_Pushed_glpi_update_GitLab------------------------------------------
#GitLab console


Do the merge in GITLAB -----------------------------


--------------------------------Pull_glpi_11.0.7_Prod-serv-----------------------------------------
#GLPI_Prod_server

cd /var/www/glpi
sudo git config --global --add safe.directory /var/www/glpi
sudo systemctl stop apache2
# Add the remote repository (only if not already configured)
# sudo git remote add origin git@192.168.150.139:jero/glpi.git
sudo git fetch origin
sudo git checkout main
sudo git reset --hard origin/main
git log --oneline -3
git status
sudo chown -R www-data:www-data /var/www/glpi
sudo -u www-data php bin/console db:update
sudo -u www-data php bin/console cache:clear
sudo chown -R www-data:www-data /var/www/glpi
sudo systemctl start apache2
sudo systemctl status apache2


