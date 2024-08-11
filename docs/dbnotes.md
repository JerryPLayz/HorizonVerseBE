# NEEDED PRAGMAS:
- `PRAGMA foreign_keys = ON;`
- `d`

# Implied Functionality from the DB:
- If a System is deleted, all stars and bodies therein will be deleted too.
- This should also impact on the entities (wormhole gates etc..) that exist within the system.

- As everything is a body, class differences 




BodyBaseTB: ParentBody_FK relates to RowID, not FK of (SystemID_FK and Name). This is to simplify the model, and will need a translation layer.

Sub-Types of Bodies:
- Stars
- Planets
- Asteroid Belt
- Trojan Belt


*Expose via API a list of all Body Types (for dropdown selection purposes)


All Objects in BodyBaseTB have an extension in sub-class tables.
- StarsTB