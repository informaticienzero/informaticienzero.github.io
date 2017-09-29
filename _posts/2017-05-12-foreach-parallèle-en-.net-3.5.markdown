---
layout: default
title: "ForEach parallèle en .NET 3.5"
date: 2017-05-12 09:34:59
permalink: /:title/
---
L'arrivée de .NET 4 avait été accompagnée de beaucoup de nouveautés et d'améliorations pour le traitement en parallèle, notamment avec la possibilité d'itérer en parallèle sur une collection. Mais voilà, je suis coincé à .NET 3.5 au travail. Cependant, grâce au travail de développeurs généreux, prêts à [partager le fruit de leur travail](https://github.com/robvolk/Helpers.Net/blob/master/Src/Helpers.Net/ParallelProcessor.cs), il existe une solution.

<!--excerpt-->

Le code suivant a été écrit par [Rob Volk](https://github.com/robvolk), que je me remercie pour partager son savoir. Thanks Rob !

{% highlight csharp %}
#region .NET extension
namespace System.Threading
{
	/// <summary>
	/// Used for emulating Parallel.ForEach from .NET 4.0
	/// </summary>
	public static class ParallelProcessor
	{
		/// <summary>
		/// Enumerates through each item in a list in parallel.
		/// Found here : https://github.com/robvolk/Helpers.Net/blob/master/Src/Helpers.Net/ParallelProcessor.cs
		/// </summary>
		public static void EachParallel<T>(this IEnumerable<T> list, Action<T> action)
		{
			// Enumerate the list so it can't change during execution.
			list = list.ToArray();
			var count = list.Count();

			if (count == 0)
			{
				return;
			}
			else if (count == 1)
			{
				// If there's only one element, just execute it.
				action(list.First());
			}
			else
			{
				// Launch each method in it's own thread.
				const int MaxHandles = 64;
				for (var offset = 0; offset <= count / MaxHandles; offset++)
				{
					// Break up the list into 64-item chunks because of a limitiation in WaitHandle.
					var chunk = list.Skip(offset * MaxHandles).Take(MaxHandles);

					// Initialize the reset events to keep track of completed threads.
					var resetEvents = new ManualResetEvent[chunk.Count()];

					// Spawn a thread for each item in the chunk.
					int i = 0;
					foreach (var item in chunk)
					{
						resetEvents[i] = new ManualResetEvent(false);
						ThreadPool.QueueUserWorkItem(new WaitCallback((object data) =>
						{
							int methodIndex = (int)((object[])data)[0];

							// Execute the method and pass in the enumerated item.
							action((T)((object[])data)[1]);

							// Tell the calling thread that we're done.
							resetEvents[methodIndex].Set();
						}), new object[] { i, item });
						i++;
					}

					// Wait for all threads to execute.
					WaitHandle.WaitAll(resetEvents);
				}
			}
		}
	}
}
#endregion
 {% endhighlight %}