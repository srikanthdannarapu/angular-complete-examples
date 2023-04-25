import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Service;

@Service
public class LargeTableService {

    @Autowired
    private JdbcTemplate jdbcTemplate1;

    public List<MyRecord> getRecordsFromDb1(int offset, int limit) {
        String sql = "SELECT * FROM mytable WHERE some_column = 'some_value' OFFSET ? ROWS FETCH NEXT ? ROWS ONLY";
        return jdbcTemplate1.query(sql, new Object[]{offset, limit}, new MyRecordRowMapper());
    }

    public List<MyRecord> getRecordsFromDb2(int limit) {
        String sql = "SELECT * FROM myother_table WHERE some_column = 'some_value' AND ROWNUM <= ?";
        return jdbcTemplate2.query(sql, new Object[]{limit}, new MyRecordRowMapper());
    }

    public List<MyRecord> getRecordsFromDb1WithOffsetAndLimit(int limit, int offset) {
        List<CompletableFuture<List<MyRecord>>> futures = new ArrayList<>();

        ExecutorService executorService = Executors.newFixedThreadPool(10);

        for (int i = 0; i < 10; i++) {
            final int currentOffset = offset + i * limit;
            CompletableFuture<List<MyRecord>> future = CompletableFuture.supplyAsync(() -> {
                return getRecordsFromDb1(currentOffset, limit);
            }, executorService);

            futures.add(future);
        }

        List<MyRecord> results = new ArrayList<>();

        for (CompletableFuture<List<MyRecord>> future : futures) {
            try {
                results.addAll(future.get());
            } catch (Exception e) {
                // Handle exceptions as needed
            }
        }

        executorService.shutdown();
        try {
            executorService.awaitTermination(Long.MAX_VALUE, TimeUnit.MILLISECONDS);
        } catch (InterruptedException e) {
            // Handle exceptions as needed
        }

        return results;
    }
}
