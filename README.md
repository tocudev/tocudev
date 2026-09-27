![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white) ![Debian](https://img.shields.io/badge/Debian-A81D33?style=flat&logo=debian&logoColor=white) ![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white) ![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat&logo=fastapi&logoColor=white) ![VS Code](https://img.shields.io/badge/VS%20Code-007ACC?style=flat&logo=visual-studio-code&logoColor=white)
```python
async def clock_in(self, ctx, job_id: int):
    async with self.bot.db.acquire() as conn:
        j = await conn.fetchrow("select * from jobs where id=$1 and guild=$2", job_id, ctx.guild.id)
        if not j:
            return await ctx.send("no job")
        if j["worker"] != ctx.author.id:
            return await ctx.send("not yours")
        if j["clocked"]:
            return await ctx.send("already on")
        await conn.execute("update jobs set clocked=true, last_in=$1 where id=$2", datetime.utcnow(), job_id)
        await ctx.send("clocked in")
        if j["missed"] >= 3:
            await conn.execute("update jobs set worker=null, clocked=false where id=$1", job_id)
            await ctx.send("fired")
```

<table style="border-collapse: collapse; width: 100%; font-family: sans-serif;">
  <tr>
    <td style="border-left: 4px solid #5865F2; background-color: #f2f3f5; padding: 12px; border-radius: 4px;">
      <strong style="color: #5865F2;">Embed Title</strong><br>
      <span style="color: #333333; font-size: 14px;">This is the description of your custom embed. You can type whatever you want here, and it will look like a Discord embed!</span>
    </td>
  </tr>
</table>