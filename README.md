# scripts


docker exec <pg> psql -U postgres -d SecurityVision -c \
  "SELECT child.\"Id\", child.\"Name\", parent.\"Name\" AS parent, parent.\"Id\" AS parent_id
   FROM conf.\"ModuleSections\" child
   JOIN conf.\"ModuleSections\" parent ON child.\"ParentId\" = parent.\"Id\"
   ORDER BY parent.\"Name\";"
И если решим делать каскад — FK тоже в схеме conf:

docker exec <pg> psql -U postgres -d SecurityVision -c "
ALTER TABLE conf.\"ModuleSections\" DROP CONSTRAINT \"FK_ModuleSections_ModuleSections_ParentId\";
ALTER TABLE conf.\"ModuleSections\" ADD CONSTRAINT \"FK_ModuleSections_ModuleSections_ParentId\"
  FOREIGN KEY (\"ParentId\") REFERENCES conf.\"ModuleSections\"(\"Id\") ON DELETE CASCADE;"
