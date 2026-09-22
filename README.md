Example code ![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
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