If you liked the project, click below and make the DroidPersistence grows increasingly

[![](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=A65MDJVWKB7XY)



# This is a new library for doing persistence in SQLite Databases used in Android Applications. #

**Wow, the new version is ready droidpersistence** <br>
New Features:<br>
- Now you can determine a primary key of your table with annotation @PrimaryKey;<br>
- No need to inherit the class BaseModel with a feature ahead;<br>
- More dynamic data modeling;<br>


<br>
With DroidPersistence:<br>
- Yout dont't need to do a DDLs for tables, the library automatically make a create and drop table sintax.<br>
- Foreign keys enabled with "on delete cascade" and "on update cascade";<br>
- @nnotations  \o/;<br>
- creates an genereted value (id) for your models;<br>

<h2>You must create a model class:</h2>
<pre><code><br>
@Table(name="GAME")<br>
public class Game {<br>
<br>
	@PrimaryKey<br>
	@Column(name="CODE")<br>
	private long code;<br>
<br>
	@Column(name="TITLE")<br>
	private String title;<br>
	<br>
	@Column(name="PRODUCTION")	<br>
	private String production;<br>
	<br>
	@Column(name="PLATFORM_ID")<br>
	@ForeignKey(tableReference="PLATFORM", onDeleteCascade=true, columnReference="CODE")<br>
	private long platform_id;<br>
	<br>
	public String getTitle() {<br>
		return title;<br>
	}<br>
	public void setTitle(String title) {<br>
		this.title = title;<br>
	}<br>
	public String getProduction() {<br>
		return production;<br>
...<br>
</code></pre>

<h2>A table definition:</h2>
<pre><code>public class GameTableDefinition extends TableDefinition&lt;Game&gt;{<br>
<br>
	public GameTableDefinition() {<br>
		super(Game.class);		<br>
	}<br>
}<br>
</code></pre>

<h2>Finally, a Dao Class:</h2>
<pre><code>public class GameDao extends DroidDao&lt;Game,Long&gt; {<br>
<br>
	public GameDao(TableDefinition&lt;Game&gt; tableDefinition, SQLiteDatabase database) {<br>
		super(Game.class, tableDefinition, database);<br>
	}<br>
}<br>
</code></pre>

<h2>Using Dao:</h2>
<pre><code>public class DataManager {<br>
<br>
	private Context context;<br>
	private SQLiteDatabase database;<br>
	private GameDao gameDao;<br>
	private PlatformDao platformDao;<br>
	<br>
	public DataManager(Context context){<br>
		setContext(context);<br>
		SQLiteOpenHelper openHelper = new OpenHelper(context, "GAMEDATABASE", null, 2);<br>
		setDatabase(openHelper.getWritableDatabase());<br>
		<br>
		this.platformDao = new PlatformDao(new PlatformTableDefinition(), database);<br>
		this.gameDao = new GameDao(new GameTableDefinition(), database);		<br>
	}<br>
...<br>
<br>
	public Game getGame(Long id){		<br>
		return getGameDao().get(id);<br>
	}<br>
<br>
	public List&lt;Game&gt; getGameList(){<br>
		return getGameDao().getAll();<br>
	}<br>
	public long saveGame(Game game){<br>
		long result = 0;		<br>
		try {<br>
			getDatabase().beginTransaction();<br>
			result = getGameDao().save(game);<br>
			getDatabase().setTransactionSuccessful();<br>
		} catch (Exception e) {<br>
			e.printStackTrace();<br>
		}		<br>
		getDatabase().endTransaction();<br>
		return result;<br>
	}<br>
...<br>
</code></pre>

<h3>Save your code, use the quick and easy DroidPersistence.</h3>
